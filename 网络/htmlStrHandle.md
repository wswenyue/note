#HTML格式的字符串处理的方法总结
>主要的字符串的拼接处理

过滤器,**将相应的字符转换为html代码**,转换包括以下字符 `<br>` `&` `\n` `<` `>` `\` 及`空格`

    private static String[][] _chars={
        {"&",   "&amp;"},
        {"\n",  "<br>"},
        {"<",   "&lt;"},
        {">",   "&gt;"},
        {" ",   "&nbsp;"},
        {"\"",  "&quot;"}
    };

    /**
     * 过滤器,将相应的字符转换为html代码,转换包括以下字符<br>
     * & \n < > \ 及空格
     * @param str
     * @return newStr
     */
    public static String filter(String str) {
        for (int i = 0; i < _chars.length; i++) {
            str = str.replaceAll(_chars[i][0], _chars[i][1]);
        }
        return str;
    }

**去除字符串中的html换行符**: `p` ,`br`, 
   
    /**
     * 去除字符串中的html换行符: p br,
     * @param source 原始字符
     * @return newStr 清除后的字符
     */
    public static String clearBR(String source) {
        String regx = "(</?(br|p)(\\s)*/?>)|(<tbody>?)";
        Pattern p = Pattern.compile(regx, Pattern.CASE_INSENSITIVE);
        Matcher m = p.matcher(source);
        StringBuffer sb = new StringBuffer();
        while (m.find()) {
            m.appendReplacement(sb, "");
        }
        m.appendTail(sb);
        return sb.toString();
    }
 
**获取字符串中子串的起始和结束位置**：

    /**
     * 从一个字串source中查找find指定的字符串，并返回匹配第一个find所指定的字符串的
     * 起始位置(不分大小)，该起始位置包含起始字符位置及结速字符位置,获取数据方法<br>
     * fin[0] -> find所指定的字符的起始位置,没匹配则返回 -1,<br>
     * fin[1] -> find所指定的字符的结速位置,没匹配则返回 -1,<br>
     * @param source 源字符串
     * @param find 所要查找的字符
     * @return fin find所指定的字符在source中的起始位置数组
     */
    public static int[] findString(String source, String find) {
        int[] fin = new int[]{-1, -1};
        Pattern p = Pattern.compile(find, Pattern.CASE_INSENSITIVE);
        Matcher m = p.matcher(source);
        if (m.find()) {
            fin[0] = m.start();
            fin[1] = m.end();
        }
        return fin;
    }
    
**查询html中指定字符串的，并高亮显示**：

    /**
     * 在一个字符串source中搜索指定的字串find，并使用默认颜色设置前景色及背
     * 景色.以加亮效果,需要使用html格式才能显示出效果，主要用于匹配查询，
     * 该方法不区分大小写
     * @see #IgnoreCaseSearch(String, String, String, String)
     * @param source 原始字符串
     * @param find 要查找的字符
     * @return newStr 加了样式的新的字符串
     */
    public static String IgnoreCaseSearch(String source, String find) {
        return IgnoreCaseSearch(source, find, null, null);
    }
    
    /**
     * 在一个字符串source中搜索指定的字串find，并使用color设置前景色及bgcolor设置背
     * 景色.需要使用html格式才能显示出效果，主要用于匹配查询，该方法不区分大小写
     * @param source 原始字符串
     * @param find 要查找的字符
     * @param color 找到后改变其前景色:格式如 red,blue,或 #FFFFFF 或null
     * @param bgcolor 找到后字串的背景色:格式如 red,blue,或 #FFFFFF 或null
     * @return newStr 加了样式的新的字符串
     */
    public static String IgnoreCaseSearch(String source, String find, String color, String bgcolor) {
        String tColor = "red";
        String tBgcolor = "yellow";
        if (color != null) tColor = color;
        if (bgcolor != null) tBgcolor = bgcolor;
        String spanStart = "<span style='color:" + tColor + ";background:" + tBgcolor +";'>";
        String spanEnd = "</span>";
        Pattern p = Pattern.compile(find, Pattern.CASE_INSENSITIVE);
        Matcher m = p.matcher(source);
        StringBuffer sb = new StringBuffer();
        while (m.find()) {
            m.appendReplacement(sb, spanStart + m.group() + spanEnd);
        }
        m.appendTail(sb);
        return sb.toString();
    }
    

**截取字符串：**
    
    /**
     * 截取字符串
     * @param str 原始字符串
     * @param size 截取的长度
     * @return 新的字符串
     */
    public static String subString(String str, int start, int size) {
        if (str.length() <= (start + size)) 
            return str.substring(start);
        return str.substring(start, start + size);
    }
    
**清除html标签：**

    /**
     * 清除Html标签
     * @param str
     * @return
     */
    public static String clearHtml(String str) {
        int flag1 = str.indexOf("<");
        int flag2 = str.indexOf(">");
        String temp = null;
        while (flag1 != -1 || flag2 != -1) {
            str = clearHtmlPrivate(str);
            flag1 = str.indexOf("<");
            flag2 = str.indexOf(">");
        }
        return str;
    }
    
    private static String clearHtmlPrivate(String str) {
        int start = str.indexOf("<");
        int end = str.indexOf(">");
        if (start != -1 || end != -1) {
            str = str.substring(0, start) + str.substring(end + 1);
        }
        return str;
    }


**根据给定的URL地址获取Html内容**：   

    /**
     * 根据给定的URL地址获取Html内容
     * @param urlStr
     * @return 
     */
    public static String getHtmlCode(String url) throws MalformedURLException, IOException {
        StringBuilder sb = new StringBuilder();
        URL u = new URL(url);
        InputStream in = u.openStream();
        InputStreamReader isr = new InputStreamReader(in);
        char[] buff = new char[2048];
        int len;
        while ((len = isr.read(buff, 0, buff.length))!= -1) {
            sb.append(buff, 0, len);
        }
        isr.close();
        return sb.toString();
    }