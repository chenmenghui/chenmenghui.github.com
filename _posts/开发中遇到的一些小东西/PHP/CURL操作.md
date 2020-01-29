---
title: CURL操作
date: 2019-12-26
categories: PHP
tags: 
- PHP
- 实用代码片段
---

其实也就是简单的封装，再一个就是继承当前浏览器的header，主要是Cookie和Referer。这两个经常会被服务端做过滤之类的处理。

刚刚犯的错误，QueryCurl::_setData()，其中「CURLOPT_POSTFIELDS」只是post传值。

工作中遇到一个问题，就是执行这个脚本的时候，进行了诸如退出这样修改Session的操作，导致一些因session不一致产生了业务上的错误。这里最好的解决方式是操作session、cookie这样的操作时，不要使用同一个浏览器。

```php
<?php


class QueryCurl
{
    private $sTargetHost = '';
    private $sParams = '';
    private $rCurlOption;
    private $mCurlResult;
    private $sCurlContent;
    private $mCurlRequestHeads;
    private $mCurlResponseHeads;

    public $bPost = true;

    const DEBUG = false;

    /**
     * Get data through get
     * @param string $sUrl
     * @param array $aParams
     * @return mixed
     */
    public function getQuery($sUrl, $aParams = [])
    {
        $this->sTargetHost = $sUrl;
        $this->sParams = http_build_query($aParams);
        $this->bPost = false;
        $this->_run();
        return $this->sCurlContent;
    }

    /**
     * Get data through post
     * @param string $sUrl
     * @param array $aParams
     * @return mixed
     */
    public function postQuery($sUrl, $aParams = [])
    {
        $this->sTargetHost = $sUrl;
        $this->sParams = http_build_query($aParams);
        $this->bPost = true;
        $this->_run();
        return $this->sCurlContent;
    }

    private function _inheritHeader()
    {
        $aHeader = [];
        foreach ($_SERVER as $key => $value) {
            if (stripos($key, 'HTTP') !== false) {
                $key = substr($key, 5);
                $aHeader[$key] = $key . ':' . $value;
            }
        };
        if (self::DEBUG) {
            file_put_contents('test_head.txt', var_export($aHeader, 1) . "\n", FILE_APPEND);
        }
        curl_setopt($this->rCurlOption, CURLOPT_HTTPHEADER, $aHeader);
    }


    private function _setData()
    {
        if ($this->bPost) {
            curl_setopt($this->rCurlOption, CURLOPT_CUSTOMREQUEST, 'POST');
            curl_setopt($this->rCurlOption, CURLOPT_POST, true);
            curl_setopt($this->rCurlOption, CURLOPT_POSTFIELDS, $this->sParams);
        } else {
            if ($this->sParams) {
                if (false === strpos($this->sTargetHost, '?')) {
                    $this->sTargetHost .= '?' . $this->sParams;
                } else {
                    $this->sTargetHost .= '&' . $this->sParams;
                }
            }
        }
    }


    private function _run()
    {
        try {
            $this->rCurlOption = curl_init();
            $this->_setData();
            $this->_inheritHeader();
            curl_setopt($this->rCurlOption, CURLOPT_URL, $this->sTargetHost);
            curl_setopt($this->rCurlOption, CURLOPT_SSL_VERIFYPEER, 0);
            curl_setopt($this->rCurlOption, CURLOPT_SSL_VERIFYHOST, 0);
            curl_setopt($this->rCurlOption, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
            curl_setopt($this->rCurlOption, CURLOPT_FOLLOWLOCATION, 1);
            curl_setopt($this->rCurlOption, CURLOPT_AUTOREFERER, 1);
            curl_setopt($this->rCurlOption, CURLOPT_TIMEOUT, 0);
            curl_setopt($this->rCurlOption, CURLOPT_HEADER, 1);
            curl_setopt($this->rCurlOption, CURLOPT_RETURNTRANSFER, 1);
            curl_setopt($this->rCurlOption, CURLINFO_HEADER_OUT, true);
            $this->mCurlResult = curl_exec($this->rCurlOption);
            $errno = curl_errno($this->rCurlOption);
            if ($errno) {
                throw new Exception($errno);
            }
            $this->mCurlRequestHeads = curl_getinfo($this->rCurlOption, CURLINFO_HEADER_OUT);
            $response_heads_size = curl_getinfo($this->rCurlOption, CURLINFO_HEADER_SIZE);
            $this->mCurlResponseHeads = $this->mCurlResponseHeads = substr($this->mCurlResult, 0, $response_heads_size);
            $this->sCurlContent = substr($this->mCurlResult, $response_heads_size);
            if (self::DEBUG) {
                file_put_contents('head.txt', $this->mCurlRequestHeads . "\n", FILE_APPEND);
                $rand = time();
                file_put_contents("result_{$rand}.txt", $this->mCurlResult);
            }
            curl_close($this->rCurlOption);
        } catch (Exception $exception) {
            // logs
            if (self::DEBUG) {
                file_put_contents('./curl_error_log.txt', var_export($exception->getMessage(), 1), FILE_APPEND);
            }
        }
    }
}
```
