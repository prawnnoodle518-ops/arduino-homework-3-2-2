# arduino-homework-3-2-2
const int buttonPin = 2; // 按鈕的 In 腳接 D2
const int ledPin = 3;    // LED 接 D3

// 狀態變數 
// 0: 恆亮 (初始)
// 1: 慢閃
// 2: 不快不慢的閃爍
// 3: 快閃
int ledState = 0;        
int buttonState = 0;     
int lastButtonState = LOW; 

// 閃爍計時變數
unsigned long previousMillis = 0; 
bool blinkState = LOW;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);
}

void loop() {
  // 1. 偵測按鈕是否被「按下」（由 LOW 變 HIGH 的瞬間）
  buttonState = digitalRead(buttonPin);
  
  if (buttonState == HIGH && lastButtonState == LOW) {
    // 每次按一下，狀態加 1，透過 % 4 讓數值在 0、1、2、3 之間循環
    // 當處於 3 時按第四下，(3 + 1) % 4 = 0，會準確回到「恆亮」狀態！
    ledState = (ledState + 1) % 4; 
    delay(50); // 簡單的按鈕彈跳消除
  }
  lastButtonState = buttonState; // 記錄這次的狀態供下次比對

  // 2. 根據當前的 ledState 執行對應動作
  unsigned long currentMillis = millis();

  switch (ledState) {
    case 0: // 狀態 0：恆亮
      digitalWrite(ledPin, HIGH);
      break;
      
    case 1: // 狀態 1：慢閃爍 (間隔 800ms)
      if (currentMillis - previousMillis >= 800) {
        previousMillis = currentMillis;
        blinkState = !blinkState;
        digitalWrite(ledPin, blinkState);
      }
      break;
      
    case 2: // 狀態 2：不快不慢的閃爍 (間隔 400ms)
      if (currentMillis - previousMillis >= 400) {
        previousMillis = currentMillis;
        blinkState = !blinkState;
        digitalWrite(ledPin, blinkState);
      }
      break;
      
    case 3: // 狀態 3：快閃爍 (間隔 150ms)
      if (currentMillis - previousMillis >= 150) {
        previousMillis = currentMillis;
        blinkState = !blinkState;
        digitalWrite(ledPin, blinkState);
      }
      break;
  }
}
