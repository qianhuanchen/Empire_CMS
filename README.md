# Empire_CMS
帝国cms
### 导航调用
+ 一级导航
```php
<?php
// 查询一级菜单
$sql = $empire->query("select classid, sonclass, classname, islast, islist from {$dbtbpre}enewsclass where bclassid=0 and showclass=0 order by myorder asc");
while ($primaryMenu = $empire->fetch($sql)) :
    $classUrl = sys_ReturnBqClassname($primaryMenu, 9); 
    $topClass = '';
    $featherClass = explode('|', $class_r[$GLOBALS['navclassid']]['featherclass']);
    $topbclassid = $featherClass[1] ? $featherClass[1] : $GLOBALS['navclassid'];
    
    // 判断是否为当前菜单
    if ($topbclassid == $primaryMenu['classid']) {
        $topClass = 'current-menu-item';
    }
    
?>
<li class="navbar-item <?=$topClass?>"><a href="<?=$classUrl?>"><?=$primaryMenu['classname']?></a></li>
<?endwhile?>
```
+ 二级导航
```php
<?php
// 查询一级菜单
$sql = $empire->query("select classid, sonclass, classname, islast, islist from {$dbtbpre}enewsclass where bclassid=0 and showclass=0 order by myorder asc");
while ($primaryMenu = $empire->fetch($sql)) :
    $classUrl = sys_ReturnBqClassname($primaryMenu, 9);
    $topClass = '';
    $featherClass = explode('|', $class_r[$GLOBALS['navclassid']]['featherclass']);
    $topbclassid = $featherClass[1] ? $featherClass[1] : $GLOBALS['navclassid'];
    // 判断是否为当前菜单
    if ($topbclassid == $primaryMenu['classid']) {
        $topClass = 'current-menu-item';
    }
?>
    <li class="<?= $topClass ?>"><a href="<?= $classUrl ?>"><?= $primaryMenu['classname'] ?></a>
        <?php if ($primaryMenu['islast'] == 0) : ?>
            <ul class="sub-menu">
                <?php
                $sql2 = $empire->query("select classid, classname from {$dbtbpre}enewsclass where bclassid={$primaryMenu['classid']} and showclass=0 order by myorder asc");
                $subMenuStr = "";
                while ($secondaryMenu = $empire->fetch($sql2)) :
                    $classUrl2 = sys_ReturnBqClassname($secondaryMenu, 9);
                ?>
                    <li><a href="<?= $classUrl2 ?>"><?= $secondaryMenu['classname'] ?></a></li>
                <?php endwhile ?>
            </ul>
        <?php endif ?>
    </li>
<?php endwhile ?>
```
### 搜索表单的调用
```php
<form action="[!--news.url--]e/search/index.php" method="post" name="searchform"> 
<input type="hidden" name="show" value="title" />
<input type="hidden" name="tempid" value="1" />
<input type="hidden" name="tbname" value="news" />
<input type="text" name="keyboard" placeholder="输入 “课程” 相关词..." class="text" /> 
<button type="submit" class="btn" value=""></button> 
</form>
```
### 高效率随机调用tag的PHP代码
+ 高效率随机调用
```php
<?php
$num=$empire->num("select tagid from {$dbtbpre}enewstags");
$randnum=100; 
$randids=''; 
$randdh=''; 
for($i=1;$i<=$randnum;$i++) 
{ 
$randids.=$randdh.rand(1,$num); 
$randdh=',';
} 
?>
[e:loop={"select tagname,tagid from phome_enewstags where tagid in ($randids)  limit $randnum",32,24,0}]
<a target="_blank" href="/tag/<?=$bqr['tagid']?>/" title="<?=$bqr['tagname']?>"><?=$bqr['tagname']?></a> | 
[/e:loop]
```
+ 正常调用
```php
[e:loop={"select tagname,tagid from phome_enewstags  limit 12 ",32,24,0}]
<a target="_blank" href="/e/tags/?tagname=<?=$bqr['tagname']?>" title="<?=$bqr['tagname']?>"><?=$bqr['tagname']?></a> | 
[/e:loop]
```
+ 随机调用
```php
[e:loop={"select tagname,tagid from phome_enewstags  order by rand() limit 12 ",32,24,0}]
<a target="_blank" href="/e/tags/?tagname=<?=$bqr['tagname']?>" title="<?=$bqr['tagname']?>"><?=$bqr['tagname']?></a> | 
[/e:loop]
```
+ 内容调用
```php
[e:loop={"select a.*,b.* from [!db.pre!]enewstags a LEFT JOIN [!db.pre!]enewstagsdata b ON a.tagid=b.tagid where b.classid='$navinfor[classid]' and b.id='$navinfor[id]' group by b.tagid order by a.num desc limit 10",0,24,0}]
<a href='<?=$public_r['newsurl']?>e/tags/?tagname=<?=$bqr['tagname']?>' title='<?=$bqr['num']?>个'><?=$bqr['tagname']?>(<?=$bqr['num']?>)</a>
[/e:loop]
```
  

