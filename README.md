# Классификация требований
Программы на языке poST и доказательства условий корректности для них можно найти [здесь](https://github.com/ivchernenko/post_vcgenerator/tree/main/case-studies)

|Система управления | Требование на естественном языке | Требование на языке DV-TRL | Pattern |
| ----------------- | -------------------------------- | -------------------------- | ------- |
| Турникет | | | |
| | Сигнал open должен быть в состоянии true не более 10 с. | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ toEnvNum s1 s ≥ 100 ∧  getVarBool s1 open = True ⟶ (∃ s3. toEnvP s3 ∧ substate s1 s3 ∧ substate s3 s ∧ toEnvNum s1 s3 ≤ 100 ∧ getVarBool s3 open = False ∧ (∀ s2. toEnvP s2 ∧ substate s1 s2 ∧ substate s2 s3 ∧ s2 ≠ s3 ⟶ getVarBool s2 open = True))) | [P1](patterns.md#pattern-1) |
| | Если турникет был закрыт (open = False) и оплата не выполнена (paid = False), то он не откроется (open = False) | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 open = False ∧ getVarBool s2 paid = False ⟶  getVarBool s2 open = False) | [P2](patterns.md#pattern-2) |
| | После появления с монетоприемника сигнала получения оплаты paid (getVarBool s1 open = False ∧  getVarBool s1 paid = False ∧ getVarBool s2 paid = True) немедленно должен быть сформирован сигнал открытия турникета open (open = True) | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 open = False ∧  getVarBool s1 paid = False ∧ getVarBool s2 paid = True ⟶ getVarBool s2 open = True) | [P2](patterns.md#pattern-2) |
| | После получения сигнала о проходе пользователя (PdOut = True) турникет должен быть закрыт (open = False) не более, чем через 1 с. | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s ≥ 10 - 1 ∧  getVarBool s1 open = True ∧ getVarBool s2 PdOut = True ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧ toEnvNum s2 s4 ≤ 10 - 1 ∧ getVarBool s4 open = False ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶ getVarBool s3 open = True))) | [P4](patterns.md#pattern-4) |
| | Сигнал open должен быть в состоянии true не менее 1 с. |  toEnvP s ∧ (∀ s1 s2 s3. toEnvP s1 ∧ toEnvP s2 ∧ toEnvP s3 ∧ substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s3 < 10 ∧ getVarBool s1 open = False ∧ getVarBool s2 open = True ⟶ getVarBool s3 open = True) | [P5](patterns.md#pattern-5) |
| | Если турникет открыт (opened = True), должен гореть светодиод (enter = True) | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ getVarBool s1 opened = True ⟶ getVarBool s1 enter = True) | [P3](patterns.md#pattern-3) |
| | После запрета прохода (getVarBool s1 enter = True ∧ getVarBool s2 enter = False) должен быть разрешена работа монетоприемника (reset = True) | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 enter = True ∧ getVarBool s2 enter = False ⟶ getVarBool s2 reset = True) | [P2](patterns.md#pattern-2) |
| | Если турникет только что открылся (getVarBool s1 open = False ∧ getVarBool s2 open = True ), он будет открыт (open = True) в течение 10 секунд или пока не пройдет пользователь (PdOut = True) | toEnvP s ∧ (∀ s1 s2. toEnvP s1 ∧ toEnvP s2 ∧ substate s1 s2 ∧ substate s2 s ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 open = False ∧ getVarBool s2 open = True ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧ toEnvNum s2 s4 ≤ 99 ∧ getVarBool s4 PdOut = True ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶ getVarBool s3 open = True)) ∨ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvNum s2 s3 ≤ 99 ⟶ getVarBool s3 open = True)) | [P7](patterns.md#pattern-7) |
|Светофор | | | |
| | Если горел красный свет (trafficLight = RED) и кнопку нажали (requestButton = PRESSED), то не более, чем через Tr с. загорится зеленый свет (trafficLight = GREEN). | toEnvP s ∧(∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s ≥ T1 ∧  getVarBool s1 trafficLight = RED ∧ getVarBool s1 requestButton = NOT_PRESSED ∧ getVarBool s2 requestButton = PRESSED ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧toEnvNum s2 s4 ≤ T1 ∧ getVarBool s4 trafficLight = GREEN ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶ getVarBool s3 trafficLight = RED))) | [P4](patterns.md#pattern-4) |
| | Если только что загорелся зеленый (etVarBool s1 trafficLight ≠ GREEN ∧ getVarBool s2 trafficLight = GREEN), то зеленый будет гореть (trafficLight = GREEN) не менее Tg с. |  toEnvP s ∧ (∀ s1 s2 s3. substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvP s3 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s3 < T2 ∧ getVarBool s1 trafficLight ≠ GREEN ∧ getVarBool s2 trafficLight = GREEN ⟶ getVarBool s3 trafficLight = GREEN) | [P5](patterns.md#pattern-5) |
| | Если только что загорелся зеленый (trafficLight = GREEN), то за время не более Tg с. он переключится на красный (trafficLight = RED). |  toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s ≥ T2+T3 ∧  getVarBool s1 trafficLight ≠ GREEN ∧ getVarBool s2 trafficLight = GREEN ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧ toEnvNum s2 s4 ≤ T2+T3 ∧ getVarBool s4 trafficLight = RED ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶ getVarBool s3 trafficLight = GREEN))) | [P4](patterns.md#pattern-4) |
| | Если загорелся красный (getVarBool s1 trafficLight ≠ RED ∧ getVarBool s2 trafficLight = RED), то красный будет гореть (trafficLight = RED) не менее Tr с. | toEnvP s ∧ (∀ s1 s2 s3. substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvP s3 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s3 < T1 ∧ getVarBool s1 trafficLight ≠ RED ∧ getVarBool s2 trafficLight = RED ⟶ getVarBool s3 trafficLight = RED) | [P5](patterns.md#pattern-5) |
| | Если только что включился красный (getVarBool s1 trafficLight ≠ RED ∧ getVarBool s2 trafficLight = RED), то он будет гореть пока не нажмут на кнопку (requestButton = PRESSED). | toEnvP s ∧ (∀ s1 s2 s3. substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvP s3 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 trafficLight ≠ RED ∧ getVarBool s2 trafficLight = RED ∧ (∀ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s3  ⟶ getVarBool s4 requestButton = NOT_PRESSED) ⟶ getVarBool s3 trafficLight = RED) | [P6](patterns.md#pattern-6) |
| Вращающаяся дверь | | | |
| | При входе пользователя (user = True) дверь начинает вращаться (rotation = True), если на перегородки не оказывается давление (pressure = False). |  toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧  toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧  getVarBool s1 rotation = False ∧ getVarBool s2 user = True ∧ getVarBool s2 pressure = False ⟶  getVarBool s2 rotation = True) |  [P2](patterns.md#pattern-2) |
||  Вращение продолжается (rotation = True), пока пользователь находится внутри пространства вращения (user = True), если на перегородки не оказывается давление (pressure = False). |  toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧  toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧  getVarBool s1 rotation = True ∧ getVarBool s2 user = True ∧ getVarBool s2 pressure = False ⟶  getVarBool s2 rotation = True) | [P2](patterns.md#pattern-2) |
| | Если пользователь покинул пространство вращения (user = False), то не более, чем через 1 с. вращение остановится (rotation = False), если за это время пользователи не появятся вновь (user = True). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s ≥ DELAY ∧ getVarBool s1 rotation = True ∧ getVarBool s2 user = False ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧ toEnvNum s2 s4 ≤ DELAY ∧ (getVarBool s4 rotation = False ∨ getVarBool s4 user = True) ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶  getVarBool s3 rotation = True ∧  getVarBool s3 user = False))) | [P4](patterns.md#pattern-4) |
| | Если на секционные перегородки оказывается давление (getVarBool s1 rotation = True ∧ getVarBool s2 pressure = True), то вращение приостанавливается (brake = True) не менее, чем на 1 с. |  toEnvP s ∧ (∀ s1 s2 s3. substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvP s3 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s3 < SUSPENSION_TIME ∧ getVarBool s1 rotation = True ∧ getVarBool s2 pressure = True ⟶ getVarBool s3 brake = True) | [P5](patterns.md#pattern-5) |
| | Если на секционные перегородки перестали оказывать давление (getVarBool s1 brake = True ∧ getVarBool s2 pressure = False), то не более, чем через 1 с. вращение возобновится (rotatio = True). |  toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s ≥ SUSPENSION_TIME ∧ getVarBool s1 brake = True ∧ getVarBool s2 pressure = False ⟶ (∃ s4. toEnvP s4 ∧ substate s2 s4 ∧ substate s4 s ∧ toEnvNum s2 s4 ≤ SUSPENSION_TIME ∧ (getVarBool s4 rotation = True ∨ getVarBool s4 pressure = True) ∧ (∀ s3. toEnvP s3 ∧ substate s2 s3 ∧ substate s3 s4 ∧ s3 ≠ s4 ⟶ getVarBool s3 rotation = False ∧ getVarBool s3 pressure = False))) | [P4](patterns.md#pattern-4) |
| | Запрещена одновременная подача сигналов rotation и brake | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ getVarBool s1 brake = True ⟶  getVarBool s1 rotation = False) |  [P3](patterns.md#pattern-3) |
| Холодильник | | | |
| | При открытии двери холодильника (getVarBool s1 fridgeDoor = CLOSED ∧ getVarBool s2 fridgeDoor = OPEN) включается освещение (lighting = True). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧  getVarBool s1 fridgeDoor = CLOSED ∧ getVarBool s2 fridgeDoor = OPEN ⟶ getVarBool s2 lighting = True) | [P2](patterns.md#pattern-2) |
| | При закрытии двери холодильника (getVarBool s1 fridgeDoor = OPEN ∧ getVarBool s2 fridgeDoor = CLOSED) выключается освещение (lighting = False). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧  getVarBool s1 fridgeDoor = OPEN ∧ getVarBool s2 fridgeDoor = CLOSED ⟶ getVarBool s2 lighting = False| [P2](patterns.md#pattern-2) |
| | Если дверь холодильника открыта (fridgeDoor = OPEN), то не более, чем через 30 с. подается сигнал (doorSignal = True), если за это время пользователь не закроет дверь (fridgeDoor = CLOSED). | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ toEnvNum s1 s ≥ OPEN_DOOR_TIME_LIMIT ∧  getVarBool s1 fridgeDoor = OPEN ⟶ (∃ s3. toEnvP s3 ∧ substate s1 s3 ∧ substate s3 s ∧ toEnvNum s1 s3 ≤ OPEN_DOOR_TIME_LIMIT ∧ (getVarBool s3 doorSignal = True ∨ getVarBool s3 fridgeDoor = CLOSED) ∧ (∀ s2. toEnvP s2 ∧ substate s1 s2 ∧ substate s2 s3 ∧ s2 ≠ s3 ⟶  getVarBool s2 doorSignal = False ∧ getVarBool s2 fridgeDoor = OPEN))) | [P1](patterns.md#pattern-1) |
| | Звуковой сигнал не подается произвольно. Сигнал подается (doorSignal = True) только если дверь открыта (fridgeDoor = OPEN) в течение не менее, чем 30 с. | toEnvP s ∧ (∀ s1 s2 s3. substate s1 s2 ∧ substate s2 s3 ∧ substate s3 s ∧ toEnvP s1 ∧ toEnvP s2 ∧  toEnvP s3 ∧ toEnvNum s1 s2 = 1 ∧ toEnvNum s2 s3 < OPEN_DOOR_TIME_LIMIT ∧ getVarBool s1 fridgeDoor = CLOSED ∧ getVarBool s2 fridgeDoor = OPEN ⟶ getVarBool s3 doorSignal = False)  [P5](patterns.md#pattern-5) |
| | Если температура в холодильнике превышает максимальную (fridgeTempGreaterMax = True), включается  компрессор (fridgeCompressor = True). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 fridgeCompressor = False ∧ getVarBool s2 fridgeTempGreaterMax = True ⟶ getVarBool s2 fridgeCompressor = True) | [P2](patterns.md#pattern-2) |
| Термопот | | | |
| | Пока требуемая температура не достигнута (getVarBool s1 boilingMode = True ∧ getVarInt s2 temperature < BOILING_POINT), крышка заблокирована (lid = LOCKED). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 boilingMode = True ∧ getVarInt s2 temperature < BOILING_POINT ⟶ getVarBool s2 lid = LOCKED) | [P2](patterns.md#pattern-2) |
| | При достижении заданной температуры (getVarBool s1 maintainingMode = True ∧ getVarBool s2 boilingButton ≠ PRESSED ∧ getVarInt s2 temperature ≥ getVarInt s2 selectedTemp) нагревательный элемент отключается (heater = False). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 maintainingMode = True ∧ getVarBool s2 boilingButton ≠ PRESSED ∧ getVarInt s2 temperature ≥ getVarInt s2 selectedTemp ⟶ getVarBool s2 heater = False) | [P2](patterns.md#pattern-2) |
| | Нагревательный элемент включается (heater = True) при понижении температуру воды более, чем на 5 градусов ниже заданной (getVarBool s1 maintainingMode = True ∧ getVarBool s2 boilingButton ≠ PRESSED ∧ getVarInt s2 temperature < getVarInt s2 selectedTemp - 5 ). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧ getVarBool s1 maintainingMode = True ∧ getVarBool s2 boilingButton ≠ PRESSED ∧ getVarInt s2 temperature < getVarInt s2 selectedTemp - 5 ⟶ getVarBool s2 heater = True) | [P2](patterns.md#pattern-2) |
| | При нажатии одной из кнопок выбора температурного режима (button1 = PRESSED) выбирается соответствующая температура (selectedTemp = TEMP1). | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ getVarBool s1 button1 = PRESSED⟶ getVarInt s1 selectedTemp = TEMP1) |  [P3](patterns.md#pattern-3) |
| | Если кнопка кипячения не нажата (boilingMode = False и maintainingMode = False), то нагрев не включится (heater = False). | toEnvP s ∧ (∀ s1 s2. substate s1 s2 ∧ substate s2 s ∧ toEnvP s1 ∧ toEnvP s2 ∧ toEnvNum s1 s2 = 1 ∧  getVarBool s1 boilingMode = False ∧ getVarBool s1 maintainingMode = False ⟶ getVarBool s2 heater = False) | [P2](patterns.md#pattern-2) |
