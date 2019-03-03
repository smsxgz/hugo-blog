---
title: "A note for command `convert`"
date: 2019-03-03T21:31:11+08:00
lastmod: 2019-03-03T21:31:11+08:00
description: ""
tags: []

mathjax: false
mathjaxEnableSingleDollar: false
mathjaxEnableAutoNumber: false

toc: false
---
{{% admonition failure failure %}}
<code style=display:block>
$ convert -density 300 example.eps example.png  
convert-im6.q16: not authorized \`example.eps' @ error/constitute.c/ReadImage/412.  
convert-im6.q16: no images defined \`example.png' @ error/convert.c/ConvertImageCommand/3258.
</code>
{{% /admonition %}}
<!--more-->
Just edit the file `/etc/ImageMagick-6/policy.xml` and modify
```
<policy domain="coder" rights="none" pattern="PS" />
<policy domain="coder" rights="none" pattern="EPI" />
<policy domain="coder" rights="none" pattern="PDF" />
<policy domain="coder" rights="none" pattern="XPS" />
```
to
```
<policy domain="coder" rights="read|write" pattern="PDF,PS" />
```

{{% figure class="center" src="/figure/3.3/example2.png" alt="example.png" title="example.png" %}}

<br>
### Reference  
[1] https://askubuntu.com/questions/1081895/trouble-with-batch-conversion-of-png-to-pdf-using-convert
