## 一些php的小程序
----
```
对于在项目中写的程序，可以以后直接拿来用
```

* [数字转为中文大写](#zhuangdaxie)
* [导出execl表格](#execl)
* [获取本月月份 第一天和最后一天](#fristdayendday)
* [将json串解析成中文](#json-china)
* [php数组根据某键值，把相同键值的合并最终生成一个新的二维数组](#newarray)
* [利用PHP 把某个数值随机分配成几份](#numincise)

<h4 id="zhuangdaxie">数字转为中文大写</h4>

```
public function get_amount($num){
        $c1 = "零壹贰叁肆伍陆柒捌玖";
        $c2 = "分角元拾佰仟万拾佰仟亿";
        $num = round($num, 2);
        $num = $num * 100;
        if (strlen($num) > 10) {
            return NULL;
        }
        $i = 0;
        $c = "";
        while (1) {
            if ($i == 0) {
                $n = substr($num, strlen($num)-1, 1);
            } else {
                $n = $num % 10;
            }
            $p1 = substr($c1, 3 * $n, 3);
            $p2 = substr($c2, 3 * $i, 3);
            if ($n != '0' || ($n == '0' && ($p2 == '亿' || $p2 == '万' || $p2 == '元'))) {
                $c = $p1 . $p2 . $c;
            } else {
                $c = $p1 . $c;
            }
            $i = $i + 1;
            $num = $num / 10;
            $num = (int)$num;
            if ($num == 0) {
                break;
            }
        }
        $j = 0;
        $slen = strlen($c);
        while ($j < $slen) {
            $m = substr($c, $j, 6);
            if ($m == '零元' || $m == '零万' || $m == '零亿' || $m == '零零') {
                $left = substr($c, 0, $j);
                $right = substr($c, $j + 3);
                $c = $left . $right;
                $j = $j-3;
                $slen = $slen-3;
            }
            $j = $j + 3;
        }

        if (substr($c, strlen($c)-3, 3) == '零') {
            $c = substr($c, 0, strlen($c)-3);
        }
        if (empty($c)) {
            return "零元整";
        }else{
            return $c;
        }
    }
    
```

<h4 id="execl">导出execl表格</h4>

```
$filename = '文档名称'. ' .csv/.xls';
 header("Cache-Control: public" );
 header("Pragma: public" );
 header("Content-type:application/vnd.ms-excel");
 header("Content-Disposition:attachment;filename={$filename}");
 header('Content-Type:APPLICATION/OCTET-STREAM');

/*添加csv表格标题*/
$titleList = array('表格标题1', '表格标题2', '表格标题3', '表格标题4', '表格标题5', '表格标题6');
$titleString = implode(',', $titleList) . "\r\n";     /*(\r\n) 表示换行*/
echo mb_convert_encoding($titleString, 'gbk', 'utf-8');    /*以utf-8的格式输出*/

foreach(){
    $dataList = array();    /*定义新数组*/
    $dataList[] = '数据'；
    $dataList[] = '数据'； 
    $dataList[] = '数据'； 
    $dataList[] = '数据'； 
    $dataList[] = '数据'； 
    $dataList[] = '数据'； 

    $activityString = implode(',', $dataList) . "\r\n";
    echo mb_convert_encoding($activityString, 'gbk', 'utf-8'); /*输出每一行数据*/
}
```


<h4 id="fristdayendday">获取本月月份第一天和最后一天</h4>
```
$date = date('Y-m-d');
$firstday = date('Y-m-01', strtotime($date));
$lastday = date('Y-m-d', strtotime("$firstday +1 month -1 day"));
```



<h4 id="json-china">将json串解析成中文</h4>
```
$str = preg_replace("#\\\u([0-9a-f]+)#ie", "iconv('UCS-2', 'UTF-8', pack('H4', '\\1'))", $str); 

实例：
$str = preg_replace("#\\\u([0-9a-f]+)#ie", "iconv('UCS-2', 'UTF-8', pack('H4', '\\1'))", '\u4ea4\u6613\u5bc6\u7801\u9519\u8bef');
```



<h4 id="newarray">php数组根据某键值，把相同键值的合并最终生成一个新的二维数组</h4>
```
foreach($sourceArray as $k=>$v) {
        $result[$v["sendto"]][] = $v; // sendto 根据你想要的相同的需要合并的键值
}
```



<h4 id="numincise">利用PHP 把某个数值 随机分配成几份</h4>
```
		 $total = 10; //数值总额 
         $num = 8; // 分成8份 
         $min = 0.005; //最小数值 
         for ($i = 1; $i < $num; $i++) { 
                $safe_total = ($total - ($num - $i) * $min) / ($num - $i); //随机安全上限 
                $money = mt_rand($min * 100, $safe_total * 100) / 100; 
                $total = $total - $money; 
                echo '第' . $i . '个数值：' . $money ; 
         } 
         echo '第' . $num . '个数值：' . $total;// 最后一份数值 
```
