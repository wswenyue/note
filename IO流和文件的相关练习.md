##三种复制方式的对比：

直接一个字节一个字节拷贝：

	 // 一个字节一个字节复制:一滴一滴转移 
	static void copyBybyte(FileInputStream fis, FileOutputStream fos)
			throws IOException {
		int b = 0;
		long start = System.currentTimeMillis();
		while ((b = fis.read()) != -1) {
			fos.write(b);
		}
		long end = System.currentTimeMillis();
		System.out.println("一个字节一个字节复制成功,共花费" + (end - start) + "ms");

	}

通过数组拷贝：
    
    	// 先将数据放入数组中复制：一瓢一瓢转移
	static void copyByBytes(FileInputStream fis, FileOutputStream fos)
			throws IOException {
		byte[] b = new byte[512];
		int len = 0;
		long start = System.currentTimeMillis();
		while ((len = fis.read(b)) != -1) {
			fos.write(b, 0, len); // 读取多少写多少
		}
		long end = System.currentTimeMillis();
		System.out.println("放入数组复制,共花费" + (end - start) + "ms");
	}
       
通过缓冲区拷贝：

	// 放入缓冲区中赋值：一桶一桶转移 
	static void copyByBuffer(FileInputStream fis, FileOutputStream fos)
			throws IOException {
		// 创建缓冲区对象
		BufferedInputStream bis = null;
		BufferedOutputStream bos = null;
		try {
			bis = new BufferedInputStream(fis);
			bos = new BufferedOutputStream(fos);

			byte[] b = new byte[512];
			int len = 0;
			long start = System.currentTimeMillis();
			while ((len = bis.read(b)) != -1) {
				bos.write(b, 0, len); // 读取多少写多少
			}
			long end = System.currentTimeMillis();
			System.out.println("放入数组复制,共花费" + (end - start) + "ms");
		} finally {
			if (bis != null) {
				bis.close();
			}
			if (bos != null) {
				bos.close();
			}
		}
	}

通过以上三种方式的对比得出：**对于大文件的拷贝使用缓冲区的复制效率最高，建议使用**

##File相关
列出指定目录下的文件及其文件夹，包括子目录中的内容

    public static void listFiles(File file) {
    		File[] files = file.listFiles(); // 获得当前目录下所有的目录和文件
    		for (File f : files) {
    			System.out.println(f.getName());  //放到这里会打印目录和文件
    			if (f.isDirectory()) {// 如果是一个目录
    				listFiles(f);	//递归调用
    			}
    		}
    }

第二种方法（对错误规避比较规范，只打印文件，不打印目录）：
        
    public static void listDirectory(File dir) throws IOException{
	    if (!dir.exists()){
	    	throw new IllegalArgumentException("目录:"+dir+"不存在");
	    }
	    if (!dir.isDirectory()){
	    	throw new IllegalArgumentException(dir+"不是目录");
	    }
	    File[] files = dir.listFiles();
	    if (files!=null && files.length>0){
		    for(File file : files){
			    if (file.isDirectory()){
			    	listDirectory(file);
			    }else {
			    	System.out.println(file);	//这里只打印文件，而不打印目录
			    }
		    }
	    }
    }
    
---
###合并文件

	public static void main(String[] args) throws IOException {
		InputStream is1 = new FileInputStream(
				"src/com/perperties/Demo9.java");
		InputStream is2 = new FileInputStream(
				"src/com/perperties/Demo10.java");
		InputStream is3 = new FileInputStream(
				"src/com/perperties/Demo11.java");
		ArrayList<InputStream> list = new ArrayList<InputStream>();
		list.add(is1);
		list.add(is2);
		list.add(is3);

		SequenceInputStream sis = new SequenceInputStream(
				Collections.enumeration(list));
		// 包装字节流到缓冲区字符流（转换流）
		BufferedReader br = new BufferedReader(new InputStreamReader(sis));
		// 创建输出流，读取合并流，保存到文件中
		BufferedWriter bw = new BufferedWriter(new FileWriter("src/demo.txt"));
		String line = null;
		while ((line = br.readLine()) != null) {
			bw.write(line);
			bw.newLine();
		}
		bw.flush();
		br.close();
		bw.close();
		System.out.println("合并成功！！");
	}

###文件分割

	public class Demo {
	// 将一个文件分割为指定大小的文件
	// 1)定义一个固定大小的字节数组
	// 2）目标位置、名称
	// 3) 循环读取固定大小的字节，然后写到目标位置

		public static void main(String[] args) throws IOException {
			File f = new File("src/1.jpg");
			InputStream is = new FileInputStream(f);
			byte[] bytes = new byte[1024 * 1024];// 1M
			int len = 0;
			int index = 0;
			while ((len = is.read(bytes)) != -1) {
				FileOutputStream fos = new FileOutputStream(new File("images",
						(index++) + f.getName()));
				fos.write(bytes, 0, len);
	
			}
			is.close();
	
		}
	}

###Properties操作

	// 只能登录3次，3次试用
	static void login() throws IOException {
		File file = new File("src/count.properties");
		if (!file.exists()) {
			file.createNewFile();
		}
		Properties ps = new Properties();
		ps.load(new FileInputStream(file));
		// 获得count的值，并给count赋初值
		String count = ps.getProperty("count", "0");
		int i = Integer.parseInt(count);
		if (i < 3) {
			i++;
			ps.setProperty("count", i + "");// 更新属性
			// 保存本次更新的属性值
			ps.store(new FileOutputStream(file), "");

		} else {
			System.err.println("已经试用超过3次");
			System.exit(0);
		}

	}

