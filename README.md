# Sound Punisher 3000

隣室の騒音を検知し、DCモーターによる振動で応答するIoTデバイス

## 概要

* Electret マイクモジュールで騒音レベルを計測
* しきい値を超えたら DC モーターを駆動し、壁を振動させる
* Raspberry Pi Pico（GPIO26=A0）を使用するシンプルな Arduino ベースプロジェクト

## ハードウェア構成

| 部品      | 型番 / 説明                         |
| ------- | ------------------------------- |
| マイコン    | Raspberry Pi Pico (GPIO26 = A0) |
| マイクアンプ  | Electret Mic Amplifier Module   |
| DC モーター | 小型 DC モーター + ドライバ               |
| 電源      | 5V USB または バッテリ                 |

回路図: `docs/circuit_diagram.png`

## ソフトウェア設定

1. `config.h` を編集し、ピン番号やしきい値を設定
2. `src/main.ino` を Arduino IDE で開く
3. ボードにアップロード

```c
#include "config.h"

void setup() {
  Serial.begin(115200);
  pinMode(NOISE_PIN, INPUT);
  pinMode(MOTOR_PIN, OUTPUT);
  stopMotor();
}

void loop() {
  int level = analogRead(NOISE_PIN);
  if (level > NOISE_THRESHOLD) {
    driveMotor();
  } else {
    stopMotor();
  }
  delay(100);
}
```

### パラメータ調整

* `NOISE_THRESHOLD`：騒音しきい値（デフォルト 100）
* シリアルモニタでリアルタイムのレベルを確認可能

## CI (GitHub Actions)

Arduino スケッチのビルド検証と lint を自動実行します。
設定は `.github/workflows/ci.yml` を参照してください。

## ライセンス

MIT

## 貢献

Issue・PR 大歓迎！
