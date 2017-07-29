title: 阿拉伯数字转汉字写法
date: 2017-07-29 22:25:27
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
找工作时看到“某团”的题目，把一个int的数字转为汉字的读法，比如123，转成一百二十三，限时20分钟。如果二十分钟做不出来，简历就不要投了。说实话，20分钟能调通的人真的不多，感觉某团还是装逼成分太多！
<!-- more -->
<The rest of contents | 余下全文>
## 题目要求
用java实现，把int的数字转为汉字读音，比如123，转成一百二十三，10020转为一万零二十

## 思路分析
中文计数的特点，以万为小节，万以内的都是以“十百千”为权位单独计数，比如一千百，一千千都是非法的。
而“十百千”这样的权位可以与“万”，“亿”进行搭配，二十亿，五千万等等。

## 中文数字的零
中文的零的使用总结起来有三个规则，
* 以10000为小节，结尾是0，不使用零，比如1020
* 以10000为小节，小节内两个非0数字之间需要零
* 小节的千位是0，若小节前无其他数字，不用零，否者用零

## 完整代码（参考算法的乐趣第四章）

```java
public class NumberTransfer {
    public final String[] chnNumChar = new String[]{"零", "一", "二", "三", "四", "五", "六", "七", "八", "九"};
    public final String[] chnUnitSection = new String[]{"", "万", "亿", "万亿"};
    public final String[] chnUnitChar = new String[]{"", "十", "百", "千"};

    @Test
    public void testNumberToChinese() {
        int[] nums = new int[]{304, 4006, 4000, 10003, 10030, 21010011, 101101101};
        for (int i = 0; i < nums.length; i++) {
            System.out.println(numberToChinese(nums[i]));
        }
    }

    public String numberToChinese(int num) {
        String strIns;
        String chnStr = "";
        int unitPos = 0;
        boolean needZero = false;
        if (num == 0)
            return "零";
        while (num > 0) {
            strIns = "";
            int section = num % 10000;
            if (needZero) {
                chnStr = chnNumChar[0] + chnStr;
            }
            // 添加节权（万，亿）
            strIns += (section != 0) ? chnUnitSection[unitPos] : chnUnitSection[0];
            chnStr = strIns + chnStr;
            // 以万为单位，求万以内的权位
            chnStr = sectionToChinese(section, chnStr);
            needZero = (section < 1000) && (section > 0);
            num = num / 10000;
            unitPos++;
        }
        return chnStr;
    }

    private String sectionToChinese(int section, String chnStr) {
        String strIns;
        int unitPos = 0;
        boolean zero = true;
        while (section > 0) {
            int v = section % 10;
            if (v == 0) {
                if (section == 0 || !zero) {
                    zero = true;// zero确保不会出现多个零
                    chnStr = chnNumChar[v] + chnStr;
                }
            } else {
                zero = false;
                strIns = chnNumChar[v]; // 此位置对应等中文数字
                strIns += chnUnitChar[unitPos];// 此位置对应的权位
                chnStr = strIns + chnStr;
            }
            unitPos++;
            section = section / 10;
        }
        return chnStr;
    }
}
```




> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
