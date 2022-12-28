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
