---
title: ClassInfo
date: 2019-09-23 22:09:44
tags: 功能代码
---

```php
<?php

/**
 * 类信息
 * Class ClassInfo
 */
class ClassInfo
{
    /**
     * 获取类信息
     * @param ReflectionClass $class
     * @return string
     */
    public static function getData(\ReflectionClass $class): string
    {
        $details = '';
        $name = $class->getName();

        // 用户自定义
        if ($class->isUserDefined()) {
            $details .= "$name is user defined\n";
        }

        // 内置函数
        if ($class->isInternal()) {
            $details .= "$name is built-in\n";
        }

        if ($class->isInterface()) {
            $details .= "$name is interface\n";
        }

        if ($class->isAbstract()) {
            $details .= "$name is an abstract class\n";
        }

        if ($class->isFinal()) {
            $details .= "$name is a final class\n";
        }

        // 实例化
        if ($class->isInstantiable()) {
            $details .= "$name can be instantiated\n";
        } else {
            $details .= "$name can not be instantiated\n";
        }

        if ($class->isCloneable()) {
            $details .= "$name can be cloned\n";
        } else {
            $details .= "$name can not be cloned\n";
        }
        return $details;
    }

    /**
     * 获取类代码
     * @param ReflectionClass $class
     * @return string
     */
    public static function getClassSource(\ReflectionClass $class): string
    {
        $path = $class->getFileName();
        $lines = file($path);
        $from = $class->getStartLine();
        $to = $class->getEndLine();
        $len = $to - $from + 1;
        return implode(array_slice($lines, $from - 1, $len));
    }

    /**
     * 类中方法信息
     * @param ReflectionClass $class
     */
    public static function methodData(\ReflectionMethod $method)
    {
        $details = '';
        $name = $method->getName();

        // 用户自定义
        if ($method->isUserDefined()) {
            $details .= "$name is user defined\n";
        }

        // 内置函数
        if ($method->isInternal()) {
            $details .= "$name is built-in\n";
        }

        if ($method->isAbstract()) {
            $details .= "$name is abstract\n";
        }

        if ($method->isPublic()) {
            $details .= "$name is public\n";
        }

        if ($method->isProtected()) {
            $details .= "$name is protected\n";
        }

        if ($method->isPrivate()) {
            $details .= "$name is private\n";
        }

        if ($method->isStatic()) {
            $details .= "$name is static\n";
        }

        if ($method->isFinal()) {
            $details .= "$name is final\n";
        }

        if ($method->isConstructor()) {
            $details .= "$name is the constructor\n";
        }

        if ($method->returnsReference()) {
            $details .= "$name returns a reference (as opposed to a value)\n";
        }

        return $details;
    }

    /**
     * 获取方法代码
     * @param ReflectionMethod $method
     * @return string
     */
    public static function getMethodSource(\ReflectionMethod $method): string
    {
        $path = $method->getFileName();
        $lines = file($path);
        $from = $method->getStartLine();
        $to = $method->getEndLine();
        $len = $to - $from + 1;
        return implode(array_slice($lines, $from - 1, $len));
    }

    /**
     * 获取类的方法中参数的信息
     * @param ReflectionParameter $arg
     * @return string
     * @throws ReflectionException
     */
    public static function argData(\ReflectionParameter $arg)
    {
        $details = '';
        $name = $arg->getName();

        // 参数位置
        $position = $arg->getPosition();
        $details .= "\$$name has position $position\n";

        // 所属类的名称
        $declaring_class = $arg->getDeclaringClass();
        $details .= "\$$name is declaring class of {$declaring_class->getName()}\n";

        // 参数的类型
        $class = $arg->getClass();
        if (!empty($class)) {
            $details .= "\$$name must be a {$class->getName()} object\n";
        }

        // 是否是引用传递
        if ($arg->isPassedByReference()) {
            $details .= "\$$name is passed by reference\n";
        }

        // 默认值
        if ($arg->isDefaultValueAvailable()) {
            $def = $arg->getDefaultValue();
            $details .= "\$$name has default: $def\n";
        }

        if ($arg->allowsNull()) {
            $details .= "\$$name can be null\n";
        }
        return $details;
    }
}
```
