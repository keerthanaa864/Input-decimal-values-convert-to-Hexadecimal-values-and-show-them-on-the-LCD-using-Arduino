#include <LiquidCrystal_I2C.h>
#include <Wire.h>
#include <Keypad.h>

// C++ code
//
const byte ROWS=4;
const byte COLS=4;
char customKey;
String decimalNum;
long decimalNumber;
String hexNumber;

char hexaKeys[ROWS][COLS]={
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'E','0','F','D'}
};
byte rowPins[ROWS]={9,8,7,6};
byte colPins[COLS]={5,4,3,2};
Keypad customKeypad= Keypad(makeKeymap(hexaKeys),rowPins,colPins,ROWS,COLS);
LiquidCrystal_I2C lcd(0x20, 16, 2);
    
void setup()
{
  Serial.begin(9600);
  lcd.backlight();
  lcd.init();
  lcd.begin(16, 2);
  lcd.clear();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("D: ");
  lcd.setCursor(0, 1);
  lcd.print("H: ");
}

void loop()
{
  customKey= customKeypad.getKey();
  if (customKey){
  if (customKey !='A' and customKey !='B' and customKey !='C' and customKey !='D' and customKey !='E' and customKey !='F'){
    decimalNum = decimalNum + String(customKey);
    decimalNumber = decimalNum.toInt();

      hexNumber = convertDecimalToHex(decimalNumber);

      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print("D: ");
      lcd.setCursor(3, 0);
      lcd.print(decimalNumber);
      lcd.setCursor(0, 1);
      lcd.print("H: ");
      lcd.setCursor(3, 1);
      lcd.print(hexNumber);
    Serial.print("Decimal: ");
    Serial.print(decimalNumber);
    Serial.print("      ");
    Serial.print("Hexadecimal: ");
    Serial.println(hexNumber);
  }
}
if (customKey == 'C'){
  decimalNum = "";
    decimalNumber = 0;
    hexNumber = "0";
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("D: ");
    lcd.setCursor(3, 0);
    lcd.print(decimalNumber);
    lcd.setCursor(0, 1);
    lcd.print("H: ");
    lcd.setCursor(3, 1);
    lcd.print(hexNumber);
  }
}


String convertDecimalToHex(long n) {

  String hexNum;
  long remainder;

  while (n > 0) {


    remainder = n % 16;
    n=n/16;

    if (remainder<10) {
      hexNum = String(remainder) + hexNum;
    }

    else {
      switch (remainder) {
     
      case 10:
      hexNum= "A" + hexNum;
      break;
      
      case 11:
      hexNum="B" + hexNum;
      break;
      
      case 12:
      hexNum= "C" + hexNum;
      break;
      
      case 13:
      hexNum= "D" + hexNum;
      break;
      
      case 14:
      hexNum= "E" + hexNum;
      break;
      
      case 15:
      hexNum= "F" + hexNum;
      break;

      }

    }
  }
    return hexNum;

}

      
      
