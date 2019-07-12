# PHP常用函数库

## 文件系统

#### file_dirSize

##### 目录大小

```php
/**
 * 获取目录总大小支持多级目录操作
 *
 * @param $dir 目录
 *
 * @return int
 */
function file_dirSize($dir) {
    $size = 0;
    foreach (glob($dir.'/*') as $file) {
        $size += is_file($file) ? filesize($file) : file_dirSize($file);
    }

    return $size;
}
```

#### file_copyDir

##### 复制目录

```php
/**
 * 复制目录支持多级目录操作
 *
 * @param $dir 原目录
 * @param $to  复制到的目录
 *
 * @return bool
 */
function file_copyDir($dir, $to) {
    is_dir($to) or mkdir($to, 0755, true);
    foreach (glob($dir.'/*') as $file) {
        $target = $to.'/'.basename($file);
        is_file($file) ? copy($file, $target) : file_copyDir($file, $target);
    }

    return true;
}
```

#### file_delDir

##### 删除目录

```php
/**
 * 删除目录支持多级目录操作
 *
 * @param $dir 目录
 *
 * @return bool
 */
function file_delDir($dir) {
    if (!is_dir($dir)) {
        return true;
    }
    foreach (glob($dir.'/*') as $file) {
        is_file($file) ? unlink($file) : file_delDir($file);
    }

    return rmdir($dir);
}
```

#### file_moveDir

##### 移动目录

```php
/**
 * 移动目录支持多级目录操作
 * *** include{file_copyDir,file_delDir}
 * @param $dir 原目录
 * @param $to  复制到的目录
 *
 * @return bool
 */
function file_moveDir($dir, $to) {
    $status = file_copyDir($dir, $to);
    if ($status) {
        return file_delDir($dir);
    } else {
        return $status;
    }
}
```

## 日期系统

####  datetime_fine_parse

##### 精细化处理时间

```php
/**
 * 精细化处理时间
 *
 * @param        $time   时间戳或者正确的时间格式
 * @param string $format 格式
 *
 * @return false|string
 */
function datetime_fine_parse($time, $format = 'm-d') {
    $timestamp = strtotime($time);
    if ($timestamp === false) {
        $timestamp = $time;
    }
    $nowtime = time();//获取现在的时间戳
    $day = strtotime(date('Y-m-d'));//获取今天凌晨的时间戳
    $pday = strtotime(date('Y-m-d', strtotime('-1 day'))); //获取昨天凌晨的时间戳
    $qday = strtotime(date('Y-m-d', strtotime('-2 day'))); //获取前天凌晨的时间戳
    $diff = $nowtime - $timestamp;
    if ($timestamp < $qday) {//判断前天以前
        return date($format, $timestamp);
    } elseif ($timestamp < $pday && $time >= $qday) {//判断前天
        return "前天";
    } elseif ($timestamp < $day && $time >= $pday) {//判断昨天
        return "昨天";
    } elseif ($diff >= 3600 && $diff < 86400) {//判断24小时以内
        return floor($diff / 3600).'小时前';
    } elseif ($diff >= 60 && $diff < 3600) {//判断1小时以内
        return floor($diff / 60).'分钟前';
    } elseif ($diff >= 0 && $diff < 60) {//判断1分钟以内
        return '刚刚';
    } else {//判断未来
        return date($format, $timestamp);
    }
}
```

