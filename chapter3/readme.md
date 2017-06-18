# Arduino教程：第三天
## 实验一：电位器PWM控制LED亮度
线性电位器是一个模拟量的电子元器件，模拟量和数字量有什么区别呢？简单的说，数字量只有0和1两种状态，对应的就是开和关，高电平和低电平。而模拟量则不一样，他的数据状态呈现线性状态例如1到1000。
所以，本次试验我们采用电位器对LED调光，这样不会像上一次按钮实验那样，亮度的变化有层级的跳跃，用电位器调光的话能够比较连贯柔和。
- 实验电路：

![arduino-day3-1-The-experimental-circuit](https://github.com/Tangchen329/ArduinoCourse/blob/master/chapter3/image/arduino-day3-1-The-experimental-circuit.png)

- 实际电路：

![arduino-day3-2-The-experimental-circuit](https://github.com/Tangchen329/ArduinoCourse/blob/master/chapter3/image/arduino-day3-2-The-experimental-circuit.png)

我们将电位器接入了arduino控制板的A0模拟量检测口，arduino的模拟接口能够测量0-5V的电压，对应的返回值为0-1024，对电压变化的测量精度相对较高。
- 程序代码：
- 作用:通过精密线性电位器PWM控制led亮度

```
*/
 
void setup()
{
  pinMode(11,OUTPUT);          //数字口要选择带#号的具有pwm功能的输出口
}
 
void loop()
{
  int n = analogRead(A0);     //读取A0模拟口的数值（0-5V 对应 0-1204取值）
  analogWrite(11,n/4);         //PWM最大取值255  所以将模拟口的取值n除以4
}

```


- 拓展：尝试控制不同变化速率

- 课后练习：一个电位器控制两个LED，一个随电位器数字增大而变亮，另一个随电位器数字增大而变暗


## 实验二：程序PWM控制LED亮度[呼吸灯]
实验电路：

![arduino-day3-3-The-experimental-circuit](https://github.com/Tangchen329/ArduinoCourse/blob/master/chapter3/image/arduino-day3-3-The-experimental-circuit.png)

因为是PWM试验，所以LED的数字接口一定要选用带#号标识的数字口，只有带#号的数字输出口才具有硬件PWM输出功能。
实际电路：

![arduino-day3-4-The-experimental-circuit](https://github.com/Tangchen329/ArduinoCourse/blob/master/chapter3/image/arduino-day3-4-The-experimental-circuit.png)
- 程序代码：
- 作用:通过循环语句控制PWM来达到呼吸灯效果

```
*/
 
void setup ()
{
  pinMode(11,OUTPUT);
}
 
void loop()
{
  for (int a=0; a<=255;a++)                //循环语句，控制PWM亮度的增加
  {
    analogWrite(11,a);
    delay(8);                             //当前亮度级别维持的时间,单位毫秒            
  }
    for (int a=255; a>=0;a--)             //循环语句，控制PWM亮度减小
  {
    analogWrite(11,a);
    delay(8);                             //当前亮度的维持的时间,单位毫秒  
  }
  delay(800);                             //完成一个循环后等待的时间,单位毫秒
}

```


- 拓展：用程序做出多个呼吸灯
- 课后练习：同时使用2组LED，1组为三色交替呼吸灯，1组为三色交替亮度递增，凸显两者的区别对比

- 扩展实验：三个电位器控制RGB灯

实验电路：

![arduino-day3-5-The-experimental-circuitpng](https://github.com/Tangchen329/ArduinoCourse/blob/master/chapter3/image/arduino-day3-5-The-experimental-circuitpng.png)

程序代码：

```
//变量声明
int ledPin1 = 9;
int ledPin2 = 10;
int ledPin3 = 11;//RGB灯的三个接脚分别接到9、10、11pwm处
int switchPin1 = 0;
int switchPin2 = 1;
int switchPin3 = 2;//三个电位计分别接入analog0,、1、2处
int value1;
int value2;
int value3;//定义三个变量用于控制灯泡变化

void setup() {
pinMode(ledPin1, OUTPUT);
pinMode(ledPin2, OUTPUT);
pinMode(ledPin3, OUTPUT);
pinMode(switchPin1, INPUT);
pinMode(switchPin2, INPUT);
pinMode(switchPin3, INPUT);
Serial.begin(9600);
}

void loop() {
value1 = analogRead(0);//读取模拟接口0的值
int v1 = map(value1,0,1023,0,255);//map缩放函数将模拟值0—1023缩放为0—255
value2 = analogRead(1);//读取模拟接口1的值
int v2 = map(value2,0,1023,0,255);
value3 = analogRead(2);//读取模拟接口2的值
int v3 = map(value3,0,1023,0,255);
Serial.println(v1,DEC);
analogWrite(ledPin1, v1);
analogWrite(ledPin2, v2);
analogWrite(ledPin3, v3);//将模拟值赋给LED
}

```
