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