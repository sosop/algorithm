**修复没有考虑到数字是2位或多位的场景**  
**说明：对输入字符串没做相应的校验，只是实现类算法。默认实现对ipv4的转换，子网掩码最大为255，尽量用的byte实现。只是算法的实现，没有过多的遵循设计上的要求**  
*分析：基本分析写到代码注释中。*


```
class IPConverter {
    // 初始化map对象，借助一个辅助空间，映射关系
	private final static Map<Character, Character> state = new HashMap<Character, Character>(2){
	   {
	       put('a', '0');
	       put('b', '1');
	   }
	};
    
    // ipv4 二进制有32位
	private final static byte BITS = 32;
    
    // ip转换成字符串
	public static String convert(String ip) {
	   String ipStr = null;
	   // TODO  题中并未要求实现
	   return ipStr;
	}
	    
	// 此符串转化成ip
	public static String reverse(String ipStr) {
	   return binaryToIPStr(binaryStr(ipStr));
	}
	
	// 先转换成二进制字符串
	private static String binaryStr(String ipStr) {
		// 借助一个长度32的字符数组
		char[] chs = new char[BITS];
	   // 把字符串当成一个栈来使用，top记录栈顶
	   byte top = 1;
	   // v1.0没有考虑到多位数字的问题，现在修补
	   String topStr = "";
	   // 输入字符串下标
	   byte index;
	   // 记录字符数组下标
	   byte i = 0;
	   // 临时字符，避免重复调用方法
	   char tmpCh;
	   // 遍历字符串时间复杂度O(length(ipStr))
	   for (index = 0; index < ipStr.length(); index ++) {
	   	tmpCh = ipStr.charAt(index);
		  	// 非数字，查找集合key，hash时间复杂度O(1)
			if (state.containsKey(tmpCh)) {
				top += i;
		      // O(top - i)
		      while(i < top) {
		          chs[i] = state.get(tmpCh);
		          i ++;
		      }
		      top = 1;
		      topstr = "";
			  } else {
			  	// 将数字记录栈顶
			  	topStr = topStr + tmpCh;
			  	top = (byte) Integer.parseInt(topStr));
			  }
	   }
	   return new String(chs);
	｝
	// 将二进制转换成十进制
	private static String binaryToIPStr(String binaryStr) {
		// ipv4字符串最大15，16会加一个点
	   StringBuffer sb = new StringBuffer(16);
	  	// 记录输入字符串下标
	   byte index = 0;
	   // 记录转换成的十进制
	   int num = 0;
		// 遍历输入二进制字符串，遇1则移位  O(1)
	   for(index = 0; index < BITS; index ++) {
	   	// 遇1移位
	      if (binaryStr.charAt(index) == state.get('b')) {
           num +=  (1 << (7 - index % 8));
       	}
       	// 8位一个数
	      if ((index + 1) % 8 == 0) {
	         sb.append(num).append('.');
	         num = 0;
	      }
	   }
	   sb.deleteCharAt(sb.length() - 1);
	   return sb.toString();
	   
	}
}
```



