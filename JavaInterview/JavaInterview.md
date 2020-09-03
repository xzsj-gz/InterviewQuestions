# java10年老QQ群：3907814   有什么问题随时在群里咨询，群里许多关于java的资料和自学视频
# java练习题

## javase
1变量、运算符和类型转换：
1.1手动输入一个学生的成绩，对这个成绩进行一次加分，加当前成绩的20%，输出加分后成绩

```java
/**
 * 手动输入一个学生的成绩，对这个成绩进行一次加分，加当前成绩的20%，输出加分后成绩
 * @param args
 */
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入成绩");
    String len = sc.next();
    if (len.matches("\\d+")) {
        int num = Integer.valueOf(len);
        num += num * 0.2;
        System.out.println(num);
    }else {
        System.out.println("请输入一个数字：");
    }

}
```



1.2商场举行店庆，抽几折打几折，
先手动输入消费金额，再输入，抽到的折扣，计算出折后价格


```java
/**
 * 先手动输入消费金额，再输入，抽到的折扣，计算出折后价格
 * @param args
 */
public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);
    System.out.println("请输入消费金额");
    String  num = scan.next();
    System.out.println("请输入抽到的折扣");
    String dis = scan.next();

    if (num.matches("\\d+") || dis.matches("\\d+")) {
        int nums = Integer.valueOf(num);
        int diss = Integer.valueOf(dis);
        int price = 0;// 累加变量
        price = nums * diss / 10;
        System.out.println("折后价格：" + price);
    }else {
        System.out.println("请输入一个数字：");
    }



}
```



1.3手动输入一个4位数，求各位数字之和

```java
/**
 * 手动输入一个4位数，求各位数字之和
 * @param args
 */
public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);
    System.out.println("请输入一个4位数");
    String num = scan.next();
    if (num.matches("\\d+")) {
        int nums = Integer.parseInt(num);
        int a = nums / 1 % 10;
        int b = nums / 10 % 10;
        int c = nums / 100 % 10;
        int d = nums / 1000 % 10;
        System.out.println(a + b + c + d);
    }else {
        System.out.println("请输入一个数字：");
    }
}

```






2分支结构：
2.1商场消费返利活动，手动输入顾客消费金额，
如果金额打8折后仍然满1000元，用户就获得200元代金券一张（不考虑多张）

```java
/**
 * 如果金额打8折后仍然满1000元，用户就获得200元代金券一张（不考虑多张）
 * @param args
 */
public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);
    System.out.println("请输入消费金额");
    String  num = scan.next();
    if (num.matches("\\d+")) {
        int nums = Integer.parseInt(num);
        double dis = nums * 0.8;// 打折后的价格
        if (dis > 1000) {
            dis = dis - 200;// 200元代金券
        }
        System.out.println(dis);
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



2.2用户输入一个年份，如果是闰年输出是闰年
（年份能被4整除，且不能被100整除，或者能被400整除的年份）

```java
/**
 * 用户输入一个年份，如果是闰年输出是闰年
 * @param args
 */
public static void main(String[] args) {
    Scanner input = new Scanner(System.in);
    System.out.println("输入年份");
    String year = input.next();

    if (year.matches("\\d+")) {
        int nums = Integer.parseInt(year);
        if (nums % 4 == 0 && nums % 100 != 0 || nums % 400 == 0) {
            System.out.println("是闰年");
        } else {
            System.out.println("不是闰年");
        }
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



2.3手动输入一个整型会员号，
如果用户输入的是4位数字，
输出登录成功，
如果用户输入的不是4位数字,
输出“您输入的会员号有误”

```java
/**
 * 手动输入一个整型会员号，
 * 如果用户输入的是4位数字，
 * 输出登录成功，
 * 如果用户输入的不是4位数字,
 * 输出“您输入的会员号有误”
 * @param args
 */
public static void main(String[] args) {

    Scanner input = new Scanner(System.in);
    System.out.println("请输入整型会员号");
    String num = input.next();


    if (num.matches("\\d+")) {
        int nums = Integer.parseInt(num);
        int i = 0;// 初始化 数字的位数
        while (nums != 0) {
            nums = nums / 10;// 被10整除 i++;
        } // 最后，这个 i 就是数字的位数
        if (num.length() != 4) {
            System.out.println("您输入的会员号有误");
        } else {
            System.out.println("登录成功!");
        }
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



2.4手动输入a，b，c三个变量的数值，
要求通过数值交换，
把输入的数值从小到大
排序放入a,b,c中，并输出

```java
/**
 * 手动输入a，b，c三个变量的数值，
 * 要求通过数值交换，
 * 把输入的数值从小到大
 * 排序放入a,b,c中，并输出
 * @param args
 */
public static void main(String[] args) {

    Scanner scanner = new Scanner(System.in);
    System.out.print("请输入第一个整数：");
    String a1 = scanner.next();
    System.out.print("请输入第二个整数：");
    String b1 = scanner.next();
    System.out.print("请输入第三个整数：");
    String c1 = scanner.next();

    if (a1.matches("\\d+") || b1.matches("\\d+") || c1.matches("\\d+")) {
        int a = Integer.parseInt(a1);
        int b = Integer.parseInt(b1);
        int c = Integer.parseInt(c1);
        int x = 0;
        if (a > b) {
            x = a;
            a = b;
            b = x;
        }
        if (a > c) {
            x = a;
            a = c;
            c = x;
        }
        if (b > c) {
            x = b;
            b = c;
            c = x;
        }
        System.out.println(a + "," + b + "," + c);
    }else {
        System.out.println("请输入一个数字：");
    }


}
```


3多分支结构
3.1商场根据会员积分打折，
2000分以内打9折，
4000分以内打8折
8000分以内打7.5折，
8000分以上打7折，
使用if-else-if结构，实现手动输入购物金额和积分，计算出应缴金额

```java
/**
 * 商场根据会员积分打折，
 * 2000分以内打9折，
 * 4000分以内打8折
 * 8000分以内打7.5折，
 * 8000分以上打7折，
 * 使用if-else-if结构，实现手动输入购物金额和积分，计算出应缴金额
 * @param args
 */
public static void main(String[] args) {

    Scanner sc = new Scanner(System.in);
    System.out.println("请输入购物金额");
    String shop1 = sc.next();
    System.out.println("请输入积分");
    String fen1 = sc.next();

    if (shop1.matches("\\d+") || fen1.matches("\\d+")) {
        int shop = Integer.parseInt(shop1);
        int fen = Integer.parseInt(fen1);
        if (fen < 2000) {
            shop *= 0.9;
            System.out.println("目前消费" + shop + "元");
        } else if (fen >= 2000 && fen <= 4000) {
            shop *= 0.8;
            System.out.println("目前消费" + shop + "元");
        } else if (fen <= 8000 && fen > 4000) {
            shop *= 0.75;
            System.out.println("目前消费" + shop + "元");
        } else if (fen > 8000) {
            shop *= 0.7;
            System.out.println("目前消费" + shop + "元");
        } else {
            System.out.println("抱歉没有折扣");
        }
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



3.2机票价格按照淡季旺季、头等舱和经济舱收费、
输入机票原价、月份和头等舱或经济舱，
其中旺季（5-10月）头等舱9折，经济舱85折，
淡季（11月到来年4月）头等舱7折，经济舱65折，
最终输出机票价格

```java
/**
 * 机票价格按照淡季旺季、头等舱和经济舱收费、
 * 输入机票原价、月份和头等舱或经济舱，
 * 其中旺季（5-10月）头等舱9折，经济舱85折，
 * 淡季（11月到来年4月）头等舱7折，经济舱65折，
 * 最终输出机票价格
 * @param args
 */
public static void main(String[] args) {

    Scanner s = new Scanner(System.in);
    System.out.println("请输入机票原价");
    String piao1 = s.next();
    System.out.println("请输入月份");
    String yue1 = s.next();
    System.out.println("请选择：1.头等" + "2.经济");
    String l1 = s.next();

    if (piao1.matches("\\d+") || yue1.matches("\\d+") || l1.matches("\\d+")) {
        int piao = Integer.parseInt(piao1);
        int yue = Integer.parseInt(yue1);
        int l = Integer.parseInt(l1);
        if (yue >= 5 && yue <= 10) {// 旺季
            if (l == 1) {
                piao *= 0.9;
            } else {
                piao *= 0.85;
            }

        } else {// 淡季
            if (l == 1) {
                piao *= 0.7;
            } else {
                piao *= 0.65;
            }

        }
        System.out.println(piao);
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



3.3选择一个形状（1长方形、2正方形、3三角形、4圆形）
根据不同的选择让用户输入不同的信息，
长方形有长和宽、
正方形有边长、
三角形有底和高、
圆形有半径，
计算输出指定形状的面积

```java
/**
 * 选择一个形状（1长方形、2正方形、3三角形、4圆形）
 * 根据不同的选择让用户输入不同的信息，
 * 长方形有长和宽、
 * 正方形有边长、
 * 三角形有底和高、
 * 圆形有半径，
 * 计算输出指定形状的面积
 * @param args
 */
public static void main(String[] args) {

    Scanner scan=new Scanner(System.in);
    System.out.println("1:长方形、2:正方形、3:三角形、4:圆形");
    String num1 = scan.next();
    if (num1.matches("\\d+")) {
        int num = Integer.parseInt(num1);
        switch(num) {
            case 1:
                System.out.println("请输入长方形的长");
                int l=scan.nextInt();
                System.out.println("请输入长方形的宽");
                int w=scan.nextInt();
                int fs=l*w;
                System.out.println("长方形面积为:"+fs);
                break;
            case 2:
                System.out.println("请输入正方形边长");
                int z=scan.nextInt();
                System.out.println("正方形面积为:"+z*z);
                break;
            case 3:
                System.out.println("请输入三角形的底长");
                int d=scan.nextInt();
                System.out.println("请输入三角形的高");
                int g=scan.nextInt();
                System.out.println("三角形面积为:"+d*g/2);
                break;
            case 4:
                System.out.println("请输入圆形的半径");
                double r=scan.nextDouble();
                double ys=3.14*r*r;
                System.out.println("圆形面积为:"+ys);
                break;
        }
    }else {
        System.out.println("请输入一个数字：");
    }
}
```



3.4输入年份和月份，输出这个月应该有多少天（使用switch结构）

```java
/**
 * 输入年份和月份，输出这个月应该有多少天（使用switch结构）
 * @param args
 */
public static void main(String[] args) {

    Scanner scan = new Scanner(System.in);
    System.out.println("请输入年份");
    String year1 = scan.next();
    System.out.println("请输入月份");
    String month1 = scan.next();


    if (month1.matches("\\d+") || year1.matches("\\d+")) {
        int month = Integer.parseInt(month1);
        int year = Integer.parseInt(year1);
        switch (month) {
            case 1:
            case 3:
            case 5:
            case 7:
            case 8:
            case 10:
            case 12:
                System.out.println("31天");
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                System.out.println("30天");
                break;
            case 2:
                if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
                    System.out.println("29天");
                } else {
                    System.out.println("28天");
                }
        }
    }else {
        System.out.println("请输入一个数字：");
    }
}
```





4循环结构（上）
4.1随机生成一个1-100之间的数字num，循环让用户输入猜这个数，
如果用户输入的数字大于num提示输入的数字比较大，
如果用户输入的数字小于num提示输入的数字比较小，
直到用户输入的数字和num相等为止，然后输出用户猜数的总次数

```java
/**
 * 随机生成一个1-100之间的数字num，循环让用户输入猜这个数，
 * 如果用户输入的数字大于num提示输入的数字比较大，
 * 如果用户输入的数字小于num提示输入的数字比较小，
 * 直到用户输入的数字和num相等为止，然后输出用户猜数的总次数
 * @param args
 */
public static void main(String[] args) {
    Scanner scan=new Scanner(System.in);
    Random ran=new Random();
    int num=ran.nextInt(100)+1;
    int n=0;
    int i=0;
    while(n!=num) {
        System.out.println("随便输入一个数字进行猜测");
        n=scan.nextInt();
        if(n>num) {
            System.out.println("你的数字比较大");
        }else if(n<num) {
            System.out.println("你的数字比较小");
        }else {
            System.out.println("终于猜对了");
        }
        i++;
    }
    System.out.println("一共猜了"+i+"次");
}
```



4.2打印出1-100之间所有不是7的倍数和不包含7的数字，并求和

```java
/**
 * 打印出1-100之间所有不是7的倍数和不包含7的数字，并求和
 * @param args
 */
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数字：");
    String num1 = sc.next();
    if (num1.matches("\\d+")) {
        int num = Integer.parseInt(num1);
        int sum = 0;
        for (int i = 1; i <= num; i++) {
            if(i%7 ==0 || i%10 == 7 || i/10 == 7){
                continue;
            }
            sum += i;
        }
        System.out.println(sum);
    }else {
        System.out.println("请输入一个数字：");
    }

}
```



4.3循环输入5个数，输完后显示这些数中有没有负数

```java
/**
 * 循环输入5个数，输完后显示这些数中有没有负数
 * @param args
 */
public static void main(String[] args) {
    Scanner scan=new Scanner(System.in);
    System.out.println("请输入5个数:");
    int flag=0;
    int i=1;
    while(i<=5) {
        int num=scan.nextInt();
        if(num<0) {
            flag=1;
        }
        i++;
    }
    if(flag==0) {
        System.out.println("没有负数");
    }else {
        System.out.println("有负数");
    }

}

```



5循环结构（下）
5.1有一个有钱的神经病，他往银行里存钱，
第一天存1元,以后每天比前一天多存50%，完成下列计算任务
1)他存到第几天，当天存的钱会超过10元

```java
public static void main(String[] args) {
    double money=1;
    int day=1;
    while(money<10) {
        money*=1.5;
        day++;
        System.out.println("day:"+day+",money:"+money);
    }
    System.out.println(day);

}
```


2)一个月（30天）后，他总共存了多少钱

```java
public static void main(String[] args) {
    double sum=0;
    double mo=1;
    for(int i=1;i<=30;i++) {
        sum+=mo;
        System.out.println("i:"+i+",money:"+mo+",sum:"+sum);
        mo*=1.5;
    }
    System.out.println(sum);
}

```


5.2有一个400米一圈的操场，一个人要跑10000米，
第一圈50秒，其后每一圈都比前一圈慢1秒，
按照这个规则计算跑完10000米需要多少秒

```java
public static void main(String[] args) {
    int round=10000/400;
    int sum=0;
    int time=50;
    for(int i=1;i<=round;i++) {
        sum+=time;
        System.out.println("时间:"+time+",多少圈:"+i+",花费时间:"+sum);
        time++;
    }
    System.out.println(sum);
}

```

5.3用户输入任意一个整数，求各位数字之和

```java
public static void main(String[] args) {
    Scanner scan=new Scanner(System.in);
    System.out.println("请输入一个数字");
    int num=scan.nextInt();
    int sum=0;//累加变量
    while(num>0) {
        sum+=num%10;
        num/=10;
    }
    System.out.println(sum);
}
```



5.4井里有一只蜗牛，他白天往上爬5米，晚上掉3.5米，井深56.7米
计算蜗牛需要多少天才能从井底到爬出来

```java
public static void main(String[] args) {
    int day=1;//天数
    double sum=0;//距离
    while(true) {
        //白天向上爬5米
        sum+=5;
        System.out.println("天:"+day+",总和:"+sum);
        if(sum>=56.7) {
            break;
        }
        //晚上掉3.5;
        sum-=3.5;
        day++;
    }
    System.out.println(day);
}

```



6循环嵌套
6.1求某个数字以内质数列表
PS：质数是只能被1和自身整除的整数


```java
/**
 * 求某个数字以内质数列表
 * @param args
 */
public static void main(String[] args) {
    int i, j;
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数字：");
    String num1 = sc.next();
    if (num1.matches("\\d+")) {
        int num = Integer.parseInt(num1);
        for (i = 1; i <= num; i++) {
            for (j = 2; j < i; j++) {
                if (i % j == 0)
                    break;
            }
            if (i == j){
                System.out.println(j + " ");
            }
        }
    }else {
        System.out.println("请输入一个数字：");
    }

}
```





7数组
7.1定义一个数组int[] nums={8,7,3,9,5,4,1}
输出数组中的最大值和最大值所在的下标

```java
public static void main(String[] args) {
    int[] nums={8,7,3,9,5,4,1};
    int max = nums[0];//默认第一个最大
    int index = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] > max) {
            max = nums[i];
            index = i;
        }
    }
    System.out.println("最大的数： "+max+" 下标： "+index);
}
```



7.2向一个长度为10的整型数组中随机生成10个0~9的随机整数，完成下列任务
1)升序输出、降序输出
2)输出总和、平均数

```java
public static void main(String[] args) {
    Random ran = new Random();
    int[] num = new int[10];
    for (int i = 0; i < num.length; i++) {
        num[i] = ran.nextInt(10);
    }
    Arrays.sort(num);
    for (int i = 0; i < num.length; i++) {
        System.out.print(num[i] + " ");
    }
    System.out.println("------升序------");
    for (int i = num.length - 1; i >= 0; i--) {
        System.out.print(num[i] + " ");
    }
    System.out.println("------降序------");

    // 总和
    int sum = 0;
    for (int i = 0; i < num.length; i++) {
        sum = num[i] + sum;
    }
    System.out.println("总和：" + sum);

    // 平均数
    int sum2 = 0;
    for (int i = 0; i < num.length; i++) {
        sum2 = num[i] + sum2;
    }
    System.out.println("平均数：" + sum2 / num.length);
}
```



7.3向一个长度为5的整型数组中随机生成5个1-10的随机整数
要求生成的数字中没有重复数


```java
public static void main(String[] args) {
    int[] nums = new int[5];
    Random ran = new Random();

    for (int i = 0; i < nums.length; i++) {
        nums[i] = ran.nextInt(10) + 1;

        for (int j = 0; j < i; j++) {
            while (nums[i] == nums[j]) {//如果重复，退回去重新生成随机数
                i--;
            }
        }
    }
    for (int i = 0; i < nums.length; i++) {
        System.out.print(nums[i] + " ");
    }
}
```



7.4（选做）向一个长度为10的整型数组中随机生成10个0~9的随机整数，完成下列任务
1)统计每个数字出现了多少次
2)输出出现次数最多的数字
3)输出只出现一次的数字中最小的数字

```java
public static void main(String[] args) {
    Random r = new Random();
    // 1. 声明源数组，包含10个0-9之间的随机数
    int[] src = new int[10];
    // 2. 声明一个标记数组，存放的是0-9，10个数字
    int[] flag = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    // 3. 声明一个用来统计标记数组中数字在源
    // 数组中的个数
    int[] count = new int[flag.length];
    // 4. 给源数组赋值0-9之间的随机数
    for (int i = 0; i < src.length; i++) {
        src[i] = r.nextInt(10);
    }
    // 5. 统计标记数组中的每个元素在源数组中
    // 有多少个即给count数组赋值
    for (int i = 0; i < flag.length; i++) {
        for (int j = 0; j < src.length; j++) {
            // 如果标记数组中的数字在源数组中有，则count+1
            if (flag[i] == src[j]) {
                count[i]++;
            }
        }
    }
    // 6. 输出src和count的数据
    System.out.println("随机产生的数据如下:");
    System.out.println(Arrays.toString(src));
    // System.out.println(Arrays.toString(count));

    // a.统计每个数字出现的次数
    // 如果count中的元素的值大于0，则输出其下标和值
    for (int i = 0; i < count.length; i++) {
        if (count[i] > 0) {
            System.out.println("数字" + i + "出现" + count[i] + "次");
        }
    }

    // b.输出出现最多次数的数字
    // 假设第一个统计的数字就是最多那个
    int max = count[0];
    int index = 0;
    for (int i = 0; i < count.length; i++) {
        if (count[i] > max) {
            max = count[i];
            index = i;
        }
    }
    System.out.println("出现次数最多的数字是" + index);
    // c. 输出只出现一次的数字中最小的数字
    for (int i = 0; i < count.length; i++) {
        if (count[i] == 1) {
            System.out.println("出现1次的数字中最小的是" + i);
            break;
        }
    }
}
```


## 基础算法

基础算法题

```java
1.打印出所有的"水仙花数"，所谓"水仙花数"是指一个三位数，其各位数字立方和等于该数本身。（例如： 153是一个"水仙花数"，因为153=1的三次方＋5的三次方＋3的三次方。）
```

```java
2.打印出所有的四位的四叶玫瑰数：如：1634，即1634=1的四次方加上6的四次方加上3的四次方加上4的四次方
```

```java
3.对于一个有正有负的整数数组，请找出总和最大的连续数列。给定一个int数组A和数组大小n，请返回最大的连续数列的和。保证n的大小小于等于3000。
测试样例：
[1,2,3,-6,1]
返回：6
```

```java
4.将一个正整数分解质因数。例如：输入90,打印出90=2*3*3*5
```

```java
5.有1、2、3、4个数字，能组成多少个互不相同且无重复数字的三位数？都是多少？
```

```java
6.一个整数，它加上100后是一个完全平方数，再加上168又是一个完全平方数，请问该数是多少？（完全平方数 :如果一个正整数 a 是某一个整数 b 的平方 .0也是完全平方数）
```

```java
7.编写一个算法来判断一个数 n 是不是快乐数。「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。如果 n 是快乐数就返回 True ；不是，则返回 False 。
```

```java
8.输出九九乘法表。
```

```java
9.猴子吃桃问题：猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个 第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天 剩下的一半零一个。 到第10天早上想再吃时，见只剩下一个桃子了。求第一天共摘了多少。
```

```java
10.打印出五位的五角星数，如：54748，即54748=5的5次方加上4的5次方加上7的5次方加上4的5次方加上8的5次方
```

```java
11.有一分数序列：2/1，3/2，5/3，8/5，13/8，21/13...求出这个数列的前20项之和。
```

```java
12.一个数如果恰好等于它的因子之和，这个数就称为"完数"。例如6=1＋2＋3.编程 找出1000以内的所有完数。完数的意思是将所有因数加起来的和等于这个数.比如28= 1+2+4+7+14
```

```java
13.古典问题：有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，假如兔子都不死，问每个月的兔子对数为多少？
```

```java
14.输入两个正整数m和n，求其最大公约数和最小公倍数。
```

```java
15.海滩上有一堆桃子，五只猴子来分。第一只猴子把这堆桃子凭据分为五份，多了一个，这只猴子把多的一个扔入海中，拿走了一份。第二只猴子把剩下的桃子又平均分成五份，又多了一个，它同样把多的一个扔入海中，拿走了一份，第三、第四、第五只猴子都是这样做的，问海滩上原来最少有多少个桃子？
```

```java
16.一球从h米高度自由落下，每次落地后反跳回原高度的一半；再落下，求它在 第n次落地时，共经过多少米？第n次反弹多高？程序分析：反弹的高度:(1/2)的n次方*h
```

```java
17.利用递归方法求10!（即10的阶乘）。
```

```java
18.有5个人坐在一起，问第五个人多少岁？他说比第4个人大2岁。问第4个人岁数，他说比第3个人大2岁。问第三个人，又说比第2人大两岁。问第2个人，说比第一个人大两岁。最后问第一个人，他说是10岁。请问第五个人多大。
```

```java
19.一个5位数，判断它是不是回文数。即12321是回文数，个位与万位相同，十位与千位相同。
```

```java
20.请输入星期几的第一个字母来判断一下是星期几，如果第一个字母一样，则继续判断第二个字母。
```

```
21.有n个人围成一圈，顺序排号。从第一个人开始报数（从1到3报数），凡报到3的人退出圈子，问最后留下的是原来第几号的那位。
```

```java
22.一个偶数总能表示为两个素数之和。
```

```java
23.七位的北斗七星数，如1741725，即每个数的7次方相加之和为1741725
```

```java
24.输入 3 个正数，判断能否构成一个三角形。
```

```java
25.编写程序解决“百钱买百鸡”问题。公鸡五钱一只，母鸡三钱一只，小鸡一钱三只，现有百钱欲买百鸡，共有多少种买法？
```

```java
26.八位的八仙花数，如24678050，即每个数的8次方相加之和为24678050
```


## 集合练习题

集合练习题
`1、创建一个ArrayList集合，输入10个数，将数从大到小输出，从小到大输出，随机输出。`


```java
2、已知有两个容器List，第一个List装有【小编,小王】,第二个容器装有【95分，94分】，请把第二个容器的94分改成95分，通过迭代器在控制打印出：
小编：95分
小王：95分
```

```java
3、使用Scanner从键盘读取一行输入，去掉其中重复字符，打印出不同的那些字符
```

```java
4、创建一个HashMap，里边存有key：username，value:password,的用户密码信息，从控制台输入一个用户和密码，程序在后台判断用户名在map中是否存在，如果不存在，就提示用户名错误，用户正确，在判断当前用户名对应的密码是否和输入的一致，如果一致就提示用户密码正确.
```

```java
5、定义一个泛型为String类型的List集合，统计该集合中每个字符（注意，不是字符串）出现的次数。例如：集合中有”abc”、”bcd”两个元素，程序最终输出结果为：“a = 1,b = 2,c = 2,d = 1”


```
```java
6、有两个list集合，l1数据有1，2，3，4 l2数据有 2，3，4，5， 将两个集合中重复的数据移除，并且把不重复的添加到第三个l3集合里边。
```

```java
7、创建一个List集合，里边有20组数据，在创建一个Map，把List中下标为0的作为map的key，下标为list.length()-1的为map的value，依次类推，最后在控制台打印出map所对应的key和value。
```

```java
8、产生10个1~20之间的随机数，要求随机数不能重复
```

```java
9、有如下需求，中国队，美国队，日本队，每个国家队下面又有乒乓球，羽毛球，篮球，每个球类下面有第一组，第二组，第三组，每个组下面有教练，队员，教练和队员的信息有用户名和性别，年龄，职位。请根据以上的需求利用List和Map以及学的集合类的知识点来完成这道题目。
```

```java
10、美国数学家维纳(N.Wiener)智力早熟，11岁就上了大学。他曾在1935~1936年应邀来中国清华大学讲学。一次，他参加某个重要会议，年轻的脸孔引人注目。于是有人询问他的年龄，他回答说：“我年龄的立方是个4位数。我年龄的4次方是个6位数。这10个数字正好包含了从0到9这10个数字，每个都恰好出现1次。”
请你推算一下，他当时到底有多年轻。
```

```java
11、123321是一个非常特殊的数，它从左边读和从右边读是一样的。输入一个正整数n， 编程求所有这样的五位和六位十进制数，满足各位数字之和等于n。
输入格式：
输入一行，包含一个正整数n。
输出格式：
按从小到大的顺序输出满足条件的整数，每个整数占一行。
```

