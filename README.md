# Beat Dragger

## 전체적인 시스템

이 게임은 마우스와 키보드를 사용해 날아오는 노트들을 판정선에서 타이밍에 맞추어 처리하는 것을 목표로 합니다. 일반적인 PC용 리듬게임의 시스템에 마우스라는 요소를 가미한 게임이라고 할 수 있습니다.

## 컨트롤

이 게임에서는 **마우스와 8키**를 사용합니다. 노트를 처리하기 위해서는 노트가 판정선에 닿았을 때 마우스를 노트의 위치로 옮기거나 노트의 위치에 해당하는 키를 눌러야 합니다.

## 노트 & 판정

앞으로 노트의 타이밍을 계산할 때는 60fps를 기준으로 한 프레임 수로 계산합니다.
노트를 처리할 때, 노트의 판정은 다음 중 하나로 이루어집니다. 각 판정에는 세부 판정인 **LATE/FAST**가 존재합니다.

- **MARVELOUS**: 최고 판정. 노트 고유의 타이밍을 기준으로 전후 1프레임까지 **MARVELOUS** 판정으로 처리됩니다. 노트 고유의 타이밍에 정확하게 처리할 시 세부 판정은 **CRITICAL**으로 처리되며, 그렇지 않을 경우 **LATE/FAST**가 표시됩니다.
- **BLAZING**: 노트 고유의 타이밍을 기준으로 전후 3프레임까지 **BLAZING** 판정으로 처리됩니다.
- **SHINY**: 노트 고유의 타이밍을 기준으로 전후 5프레임까지 **SHINY** 판정으로 처리됩니다.
- **DAMP**: 노트 고유의 타이밍을 기준으로 전후 6프레임까지 **DAMP** 판정으로 처리됩니다.
- **BROKEN**: 최하 판정. 노트 고유의 타이밍을 기준으로 전후 6프레임 이내에 노트를 처리하지 못하였을 경우 **BROKEN** 판정으로 처리됩니다.

이 게임에 존재하는 노트를 목록으로 정리하면 다음과 같습니다.

- **NORMAL**: 이 노트는 마우스로 처리해야 합니다. 노트 고유의 타이밍보다 3프레임까지 일찍 처리할 경우, 최고 판정인 **MARVELOUS CRITICAL**로 처리됩니다. 노트 고유의 타이밍보다 늦게 처리할 경우, 상술한 판정을 적용합니다. 노트 고유의 타이밍보다 3프레임 이상 일찍 처리할 경우, 처리되지 않습니다.
- **CRIT**: 이 노트는 키보드로 처리해야 합니다. 노트의 판정은 상술한 판정을 그대로 적용합니다.
- **FLICK**: 이 노트는 마우스로 처리해야 합니다. 노트 고유의 타이밍에 맞추어 마우스를 가로 방향으로 빠르게 쓸어넘겨 처리합니다. 노트의 특성상 각 판정에 전후 1프레임의 추가적 여유를 둡니다.
- **HOLD**: 이 노트는 시작부/지속부/종료부로 이루어져 있으며, 키보드로 처리해야 합니다. 시작부는 **CRIT** 노트이며, 종료부는 **NORMAL** 또는 **CRIT** 노트가 될 수 있습니다. 지속부는 HOLD 노트의 일정 간격마다 설치되어 있으며, 노트 고유의 타이밍을 기준으로 노트의 위치에 마우스가 있는지, 또는 키가 눌려 있는지 확인합니다. 입력이 확인되었을 경우 **MARVELOUS CRITICAL**, 그렇지 않을 경우 **BROKEN**으로 처리됩니다. HOLD 노트의 종료부에 위치한 NORMAL 노트는 키보드로 처리해야 합니다.
- **DRAG**: 이 노트는 HOLD 노트와 같이 시작부/지속부/종료부로 이루어져 있으며, 마우스로 처리해야 합니다. 시작부는 **NORMAL** 노트이며, 종료부는 **NORMAL** 또는 **FLICK** 노트가 될 수 있습니다.
- **BREAK**: 이 노트의 고유 타이밍에, 노트의 위치에 해당하는 키가 눌려 있거나 노트의 위치에 마우스가 있을 경우, 해당 노트는 **BROKEN**으로 처리되며, 그렇지 않을 경우 **MARVELOUS CRITICAL**으로 처리됩니다. 일반적인 노트의 반대 개념이라고 생각하시면 편합니다.

이상의 내용을 표로 정리하면, 다음과 같이 나타낼 수 있습니다.

|노트|~-8|-7|-6|-5|-4|-3|-2|-1|0|+1|+2|+3|+4|+5|+6|+7|+8~|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|**NORMAL**|-|-|-|-|-|MARVELOUS CRITICAL|MARVELOUS CRITICAL|MARVELOUS CRITICAL|MARVELOUS CRITICAL|MARVELOUS LATE|BLAZING LATE|BLAZING LATE|SHINY LATE|SHINY LATE|DAMP LATE|BROKEN|BROKEN|
|**CRIT**|BROKEN|BROKEN|DAMP FAST|SHINY FAST|SHINY FAST|BLAZING FAST|BLAZING FAST|MARVELOUS FAST|MARVELOUS CRITICAL|MARVELOUS LATE|BLAZING LATE|BLAZING LATE|SHINY LATE|SHINY LATE|DAMP LATE|BROKEN|BROKEN|
|**FLICK**|BROKEN|DAMP FAST|SHINY FAST|SHINY FAST|BLAZING FAST|BLAZING FAST|MARVELOUS FAST|MARVELOUS CRITICAL|MARVELOUS CRITICAL|MARVELOUS CRITICAL|MARVELOUS LATE|BLAZING LATE|BLAZING LATE|SHINY LATE|SHINY LATE|DAMP LATE|BROKEN|

## 스코어링

이 게임에서는 각 채보의 만점을 **999999**점으로 합니다. 이는 모든 노트를 **MARVELOUS CRITICAL** 판정으로 처리하였을 때의 점수입니다.

노트의 종류에 따라 각 노트에 할당되는 점수의 가중치는 다음과 같습니다.

- **CRIT** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 시, **NORMAL** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 때의 1.5배에 해당하는 점수를 받습니다.
- **FLICK** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 시, **NORMAL** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 때의 2배에 해당하는 점수를 받습니다.
- **HOLD** 노트와 **DRAG** 노트의 경우, 노트의 지속부에서는 채보의 BPM에 따른 시간 간격마다 판정이 이루어집니다. 이 간격을 틱(tick)이라 하면, 노트의 지속부에서는 MARVELOUS CRITICAL 상태를 유지하고 있다는 전제 하에 1틱마다 **NORMAL** 노트를 MARVELOUS CRITICAL으로 처리하였을 때의 0.2배에 해당하는 점수를 부여합니다. 노트의 시작부와 종료부에서는 시작부, 종료부에 위치한 노트의 배점을 따라갑니다.
- **BREAK** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 시, **NORMAL** 노트를 MARVELOUS CRITICAL 판정으로 처리하였을 때와 동일한 점수를 받습니다.

판정에 따라 부여되는 점수는 다음과 같습니다.

- **MARVELOUS** 판정 (LATE/FAST): **MARVELOUS CRITICAL** 판정으로 처리하였을 때의 점수 - 1
- **BLAZING** 판정: **MARVELOUS CRITICAL** 판정으로 처리하였을 때의 점수 \* 0.8
- **SHINY** 판정: **MARVELOUS CRITICAL** 판정으로 처리하였을 때의 점수 \* 0.5
- **DAMP** 판정: **MARVELOUS CRITICAL** 판정으로 처리하였을 때의 점수 \* 0.1
- **BROKEN** 판정: 0

또한 채보 플레이가 끝나고, 최종적으로 획득한 점수에 따라 해당 채보에서의 플레이어의 랭크가 결정됩니다.

- **S++**: 999999
- **S+** : 950000 ~ 999998
- **S** : 900000 ~ 949999
- **A+** : 850000 ~ 899999
- **A** : 800000 ~ 849999
- **B+** : 750000 ~ 799999
- **B** : 700000 ~ 749999
- **C+** : 650000 ~ 699999
- **C** : 600000 ~ 649999
- **D+** : 550000 ~ 599999
- **D** : 500000 ~ 549999
- **F** : 0 ~ 499999

## 콤보 시스템

이 게임에서 콤보는, **BLAZING** 이상의 판정으로 연속으로 처리한 노트의 개수를 의미합니다. 즉, **SHINY** 이하의 판정으로 노트를 처리할 시 콤보가 끊깁니다.

이 게임에서는 채보 플레이 중 현재 콤보 수를 화면에 표시합니다. 채보의 플레이가 끝나면, 결과 확인 창에서는 해당 플레이에서의 콤보의 최댓값을 표시합니다.

모든 노트를 **BLAZING** 이상의 판정으로 처리하였을 때, 즉, 채보 플레이 도중 한 번도 콤보가 끊기지 않았을 때, 해당 채보는 **FULL COMBO**로 처리됩니다. 또한 모든 노트를 **MARVELOUS** 판정으로 처리하였을 때, 해당 채보는 **SUPER STAR**으로 처리됩니다. 모든 노트를 **MARVELOUS CRITICAL** 판정으로 처리하였을 때, 해당 채보는 **SUPER STAR CRITICAL**으로 처리됩니다.

**FULL COMBO**, **SUPER STAR**, **SUPER STAR CRITICAL** 달성 시 플레이어에게는 특정한 보상이 주어집니다.
