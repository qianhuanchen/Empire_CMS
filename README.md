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
