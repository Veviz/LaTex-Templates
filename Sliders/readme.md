## Notice:

该幻灯片模版是在[makokal](https://github.com/makokal/beamer-themes/tree/master/JUB)给的beamer模版-JUB的基础上改进的。

主要是我师兄[Felix](https://github.com/uraplutonium)的进行的修改。

注意，该模版在使用过程中，beamer生成pdf后，自动创建的目录会出现中文乱码，因为beamer不能使用dvipdfmx来生成pdf所以对中文标签的支持不能通过dvipdfmx来完成。CJKutf8可以很好的完成中文标签tounicode的转换，但是beamer.cls中的定义存在问题，我们需要对beamer.cls文件进行修改。

linux：/usr/local/texlive/2015/texmf-dist/tex/latex/beamer/beamer.cls

windows：texlive 2015的安装目录\2015\texmf-dist\tex\latex\beamer\beamer.cls，找到

```
\DeclareOptionBeamer{CJK}{\ExecuteOptionsBeamer{cjk}}
\DeclareOptionBeamer{cjk}{
  \def\beamer@hypercjk{\hypersetup{CJKbookmarks=true}}
  \def\beamer@activecjk{
    % Activate all >=0x80 characters.
    \count@=127
    \@whilenum\count@<254 \do{%
      \advance\count@ by 1
      \lccode`\~=\count@
      \catcode\count@=\active
      \lowercase{\def~{\kern1ex}}
    }
  }
}
```

将其中的一句：`% Activate all >=127characters.` 改为：`% Activate all >=0x80 characters.`

同时在上面一段代码后面加入一段代码即可：

```
\DeclareOptionBeamer{CJKutf8}{\ExecuteOptionsBeamer{cjkutf8}}
\DeclareOptionBeamer{cjkutf8}{%
\PassOptionsToPackage{unicode}{hyperref}
  \def\beamer@activecjk{
     % Activate all characters >= 0x80 characters.
   \count@=127
   \@whilenum\count@<254 \do{%
   \advance\count@ by 1
   \lccode`\~=\count@
   \catcode\count@=\active
   \lowercase{\def~{\kern1ex}}
       }
    }
}
```

