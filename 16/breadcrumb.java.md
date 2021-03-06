处理面包屑
===========

场景
----
在实际的测试脚本中，有可能需要处理面包屑。处理面包屑主要是获取其层级关系，以及获得当前的层级。一般来说当前层级都不会是链接，而父层级则基本是以链接，所以处理面包屑的思路就很明显了。找到面包屑所在的div或ul，然后再通过该div或ul找到下面的所有链接，这些链接就是父层级。最后不是链接的部分就应该是当前层级了。

代码
----
### breadcrumb.html
```
	<html>
		<head>
			<meta http-equiv="content-type" content="text/html;charset=utf-8" />
			<title>breadcrumb</title>		
			<script type="text/javascript" async="" src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
			<link href="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.min.css" rel="stylesheet" />		
			<script type="text/javascript">
				$(document).ready(
					function(){
						
					}
				);
			</script>
		</head>
		<body>
			<h3>breadcrumb</h3>
			<div class="row-fluid">
				<div class="span3">		
					<ul class="breadcrumb">
						<li><a href="#">Home</a> <span class="divider">/</span></li>
						<li><a href="#">Library</a> <span class="divider">/</span></li>
						<li class="active">Data</li>
					</ul>
				</div>		
			</div>		
		</body>
		<script src="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/js/bootstrap.min.js"></script>
	</html>

```

### breadcrumb.java
```
	import java.io.File;
	import java.util.List;

	import org.openqa.selenium.By;
	import org.openqa.selenium.WebDriver;
	import org.openqa.selenium.WebElement;
	import org.openqa.selenium.chrome.ChromeDriver;


	public class Breadcrumb {

		public static void main(String[] args) throws InterruptedException {
			WebDriver dr = new ChromeDriver();
			
			File file = new File("src/breadcrumb.html");
			String filePath = "file:///" + file.getAbsolutePath();
			System.out.printf("now accesss %s \n", filePath);
			
			dr.get(filePath);
			Thread.sleep(1000);
			
	//		获得其父层级
			List<WebElement> ancestors = dr.findElement(By.className("breadcrumb")).findElements(By.tagName("a"));
			for(WebElement link : ancestors){
				System.out.println(link.getText());
			}
			
	//		获取当前层级
	//		由于页面上可能有很多class为active的元素
	//		所以使用层级定位最为保险
			WebElement current = dr.findElement(By.className("breadcrumb")).findElement(By.className("active"));
			System.out.println(current.getText());
			
			Thread.sleep(1000);
			System.out.println("browser will be close");
			dr.quit();	
		}

	}


```

