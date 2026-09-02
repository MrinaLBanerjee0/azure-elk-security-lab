# Detection Evidence

## Rule Overview

![Detection Rules Overview](detection-rules-overview.png)

The screenshot shows the configured custom rules. Names are historical labels and do not replace inspection of the exported query.

## SSH Rule

![SSH Rule Execution](ssh-rule-execution.png)

![SSH Alert](ssh-alert.png)

These images prove successful execution and alert creation for `soc ssh brute force`.

## Historically Named Windows Rule

![Windows Rule Execution](rdp-rule-execution.png)

![Windows Alert Details](rdp-alert-details.png)

The rule was named `win Rdp brute force`, but its export filtered Event ID `4625`, agent `win`, and user `Mrinal` without checking Logon Type `10`. These images prove execution and alert creation, not RDP specificity.

## Alert Volume

![Alerts Overview](alerts-overview.png)

The high volume from the historically named Windows rule demonstrates that tuning and suppression work remained.
