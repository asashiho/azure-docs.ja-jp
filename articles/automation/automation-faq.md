---
title: Azure Automation の FAQ | Microsoft Docs
description: Azure Automation についてよく寄せられる質問の回答です。
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 3fa29f3df5f0434c4c61e8d12adbb3f55156a29f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405959"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Automation についてよく寄せられる質問

この Microsoft FAQ は、Azure Automation についてよく寄せられる質問の一覧です。 その機能について追加のご質問がある場合は、ディスカッション フォーラムにアクセスして質問を投稿してください。 よく寄せられる質問については、すばやく簡単に見つけることができるように、この記事に追加していきます。

>[!NOTE]
>この記事は、新しい Azure PowerShell Az モジュールを使用するために更新されました。 AzureRM モジュールはまだ使用でき、少なくとも 2020 年 12 月までは引き続きバグ修正が行われます。 Az モジュールと AzureRM の互換性の詳細については、「[Introducing the new Azure PowerShell Az module (新しい Azure PowerShell Az モジュールの概要)](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)」を参照してください。 Hybrid Runbook Worker での Az モジュールのインストール手順については、「[Azure PowerShell モジュールのインストール](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)」を参照してください。 Automation アカウントについては、「[Azure Automation の Azure PowerShell モジュールを更新する方法](automation-update-azure-modules.md)」に従って、モジュールを最新バージョンに更新できます。

## <a name="update-management-solution"></a>Update Management ソリューション

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>予期しない OS レベルのアップグレードを防止することはできますか?

Red Hat Enterprise Linux など、Linux バリエーションの中には、パッケージを使用して OS レベルのアップグレードが発生することがあります。 これにより、OS バージョン番号の変更を伴う Update Management が実行される可能性があります。 Update Management でパッケージを更新するときに使用される方法は、管理者がローカルで Linux マシンに使用するものと同じであるため、この動作は意図的なものです。

Update Management のデプロイによる OS バージョンの更新を回避するには、**除外**機能を使用します。

Red Hat Enterprise Linux で、除外するパッケージ名は `redhat-release-server.x86_64` です。

### <a name="why-arent-criticalsecurity-updates-applied"></a>重要な更新プログラムまたはセキュリティ更新プログラムが適用されないのはなぜですか?

更新プログラムを Linux マシンにデプロイする場合、更新プログラムの分類を選択できます。 このオプションにより、更新プログラムがフィルター処理され、指定した条件を満たすものに絞られます。 このフィルターは、更新プログラムが展開されるときに、マシンに対してローカルに適用されます。

Update Management では更新プログラムの強化がクラウドで実行されるため、Update Management で一部の更新プログラムに対して、セキュリティへの影響ありというフラグを設定できますが、ローカル マシンにはその情報がありません。 重大な更新プログラムを Linux マシンに適用する場合、そのマシンにセキュリティへの影響ありとマークされていない更新プログラムが存在する可能性があり、そのため、これらの更新プログラムは適用されません。 ただし、Update Management には関連する更新プログラムに関する追加情報があるため、そのマシンは引き続き非対応として報告されることがあります。

更新プログラムの分類ごとの更新プログラムのデプロイは、RTM バージョンの CentOS では動作しません。 CentOS に更新プログラムを正しくデプロイするには、更新プログラムが確実に適用されるように、すべての分類を選択します。 SUSE では、分類として **[他の更新プログラム]** のみを選択すると、zypper (パッケージ マネージャー) に関連するセキュリティ更新プログラムまたはその依存関係がまず必要になる場合は、いくつかの追加のセキュリティ更新プログラムがインストールされることがあります。 この動作は、zypper の制限です。 場合によっては、更新プログラムのデプロイを再実行してから、更新プログラムのログでデプロイを確認する必要があります。

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Azure テナントの境界を越えて更新プログラムをデプロイすることはできますか?

Update Management への報告を行う別の Azure テナントに、修正プログラムを適用する必要があるマシンが存在する場合は、次の対処法を使用して、それらをスケジュールを設定する必要があります。 スケジュールを作成するには、`ForUpdateConfiguration` パラメーターを指定して [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) コマンドレットを使用します。 [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) コマンドレットを使用して、他のテナントのマシンを `NonAzureComputer` パラメーターに渡すことができます。 次の例は、その方法を示したものです。

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>次のステップ

こちらでご質問の回答が見つからない場合は、次のソースで他の質問と回答を参照できます。

- [Azure Automation](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)
- [フィードバック フォーラム](https://feedback.azure.com/forums/905242-update-management)
