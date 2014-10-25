ȫ־A20�������ײ������һЩ��װ������free pascal����װ�ࡣ
ʹ�÷������ڹ�������Ӹ������ڵ��ļ���·�����ɡ�

һ��ÿһ�����蹦�ܷ�Ϊ�����ַ�װ��һ����ֱ�Ӷ�ĳһPin��ͨ���Ĳ����࣬ʵ�ֶԳ��ù��ܵķ�װ����һ���ǶԸ���������ļĴ��������࣬������ǰ��û�з�װ���Ĺ��ܽ������á�

Ŀǰʵ���˶�GPIO��LRADC��PWM�ķ�װ����������½����ɣ��������ڳ������Ӷ��жϵ���Ӧ���ܡ�

��������ڿ���̨���н�����޽�������о��ɣ����Ҹ��ݱ������Ĳ�ͬ��Ҳ�����ڲ�ͬ�Ĳ���ϵͳ��ֻ����ٵĴ���Ķ��������ò�ͬ�ĵ�Ԫ�ȡ�

ȫ־����ϵ�еĴ�����Ҳ��ʹ�ø��࣬��A10�ȣ�ֻ��Ҫ���ݲ���Ĳ��ֽ��иĶ������߼̳�����ʵ�֡�

���ߣ�tjCFeng
���䣺tjCFeng@163.com

���ӣ�
1.TGPIOGROUP

[code]
uses GPIO;

var PHG: TGPIOGROUP;
begin
  PHG:= TGPIOGROUP.Create(PH); //����
  PHG.GPIO_DAT^:= PHG.GPIO_DAT^ or ($1 shl 24); //���üĴ�����ֵ
  PHG.Free; //�ͷ�
end;
[/code]

2.TGPIO

[code]
uses GPIO;

begin
  with TGPIO.Create(PH, 24) do
  begin
    Fun:= Fun1; //����PH24Ϊ���
    Data:= True; //���øߵ�ƽ
    Sleep(1000);
    Reverse; //��ת��ƽ
    Free; //�ͷ�
  end;
end;
[/code]

��

[code]
var PH24: TGPIO;
begin
  PH24:= TGPIO.Create(PH, 24);
  PH24.Fun:= Fun1;
  PH24.Reverse;
  PH24.Free;
end;
[/code]

3.LRADC

[code]
uses LRADC;

var ADC0: TLRADC; Data: Byte;
begin
  ADC0:= TLRADC.Create(LRADC_0); //����LRADCͨ��0
  TLRADCGROUP.Instance.ClearAllPending; //�������δ���жϣ������еĹ���
  ADC0.INTs:= [ADCDATA, KEYDOWN, KEYUP]; //������Ҫ��Ӧ���ж�����
  TLRADCGROUP.Instance.Start; //����LRADC�������еĹ���
  Data:= ADC0.Data; //��ȡLRADCͨ��0��ֵ0~64
  TLRADCGROUP.Instance.Stop; //ֹͣLRADC�������еĹ���
  ADC0.Free; //�ͷ�
end;
[/code]

4.PWM

[code]
uses PWM;

var PWM1: TPWM;
begin
  PWM1:= TPWM.Create(PWM_1); //����PWMͨ��1
  with PWM1 do
  begin
    Prescale:= P960; //����Ԥ��Ƶ
    Cycle:= 6000; //�������ڼ���
    Duty:= 1000;  //����ռ�ձȼ���
    Start; //��ʼPWM���
    Sleep(3000);
    Stop; //ֹͣPWM���
    Free; //�ͷ�
  end;
end;
[/code]

5.Timer

[code]
uses Timer;

var Timer0: TTimer;
begin
  Timer0:= TTimer.Create(Timer_0);
  with Timer0 do
  begin
    Prescal:= Div4;
    CNT:= 6000000;
    CUR:= 0;
    Start;
    while not Timer0.INT do ;
    //ִ�е�������1��
    Stop;
    Free;
  end;
end;
[/code]

6.RTC

[code]
uses RTC;

var DT: TYMDHNSW;
begin
  with DT do
  begin
    Year:= 14;
    Month:= 10;
    Day:= 20;

    Hour:= 9;
    Minute:= 30;
    Second:= 0;

    Week:= Monday;
  end;
  TRTC.Instance.DateTime:= DT;

  FillChar(DT, SizeOf(TYMDHNSW), 0);
  DT:= TRTC.Instance.DateTime;
end;
[/code]

7.General Purpose

[code]
uses GP;

var Data: LongWord;
begin
  TGP.Instance.TMR_GP[0]^:= 123456789;
  Data:= TGP.Instance.TMR_GP[10]^;
end;
[/code]

8.TWI
[code]
uses TWI;

var TWI0: TTWI; Data: Byte;
begin
  TWI0:= TTWI.Create(TWI_0);
  TWI0.Write($34, $35, $83);
  TWI0.Read($34, $35, Data);
  TWI0.Free;
end;
[/code]

��ʷ�汾��
2014.10.18 v0.5 ����TWI��װ�࣬��������bug
2014.10.16 v0.3 ����General Purpose��װ��
2014.10.15 v0.3 ����RTC��װ�࣬�������ֱ���λ�����bug
2014.10.14 v0.2 ����Timer��װ��
2014.10.03 v0.1 ���GPIO��LRADC��PWM�ķ�װ��
