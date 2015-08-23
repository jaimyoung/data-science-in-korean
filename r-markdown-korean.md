# 한글을 포함한 R Markdown 컴파일
맥 OSX상의 R Studio에서 
한글을 포함한
[R Markdown](http://rmarkdown.rstudio.com/)문서를
컴파일하려면 HTML은 문제없지만 Word / PDF 출력은
한글이 깨어지게 된다.
그 이유는 PDF의 경우
R Markdown 문서 컴파일시 호출하는 xelatex / pandoc 명령어에 
적절한 한글 폰트가 지정되지 않아서이고,
DOCX의 경우는 사용되는 템플릿에 (역시) 적절한 한글 폰트가 
지정되지 않아서이다.
근본적인 해결책이 OSX + R Studio + xelatex + Pandoc 팀으로부터
제공되기 전까지 임시 방편으로 
한글을 컴파일하는 방법을 알아보자.


## 준비작업 (한번만 해주면 된다)
일단 LaTex / xeLaTex를 설치하자. 
<http://wiki.ktug.org/wiki/wiki.php/설치하기MacOSX/MacTeX>
의 설명을 따르면 된다.
특히 nanum 폰트를 다운받도록 한다.

    sudo tlmgr repository add http://ftp.ktug.org/KTUG/texlive/tlnet ktug
    sudo tlmgr pinning add ktug "*"
    sudo tlmgr install nanumttf hcr-lvt
    sudo tlmgr update --all --self



## PDF로 컴파일하기
다음 내용을 Rmd 화일 상단에 추가한다.
```yaml
---
output:
  pdf_document:
    latex_engine: xelatex
mainfont: NanumGothic
---
```
"Knit PDF" 결과가 한글을 제대로 보여줄 것이다.


만약 R 커맨드 라인에서 화일을 컴파일 하려면 다음처럼 하면 된다:
```r
library(rmarkdown)
render("test-template.Rmd", output_format =
         pdf_document(toc=TRUE, latex_engine = 'xelatex',
                      pandoc_args = c("--variable", "mainfont='NanumGothic'")),
       clean = TRUE)
system("open test-template.pdf")
```

## DOCX로 컴파일하기
소스 작업 디렉토리에 다음 화일을 다운받는다:
[`reference_korean_friendly.docx`](<https://github.com/Jaimyoung/data-science-in-korean/blob/master/korean_template.docx?raw=true>)

다음 내용을 Rmd 화일 상단에 추가한다.
```yaml
---
output:
  word_document:
    highlight: tango
    reference_docx: korean-template.docx
---
```
"Knit DOCX" 결과가 한글을 제대로 보여줄 것이다.


만약 R 커맨드 라인에서 화일을 컴파일 하려면 다음처럼 하면 된다:
```r
library(rmarkdown)
render("test-template.Rmd", output_format =
         word_document(reference_docx = "korean-template.docx"),
       clean = TRUE)
system("open test-template.docx")
```


## `korean-template.docx` 만들어진 방법
궁금한 분을 위해 `korean-template.docx`가 어떻게 만들어졌는지
소개한다.

1. R 스튜디오에서 `template-proto.Rmd`를 연다
1. `Knit Word` 명령으로 `template-proto.docx` 화일을 생성한다.
1. MS Word 에서 `template-proto.docx` 를 연다. 
    만약 영문판 OSX를 사용한다면 폰트가 다 깨져서 보일 것이다.
1. 다음의 스타일의 폰트를 "나눔고딕"으로 바꾼다.
    1. Normal
        - 많은 스타일들이 "Style based on:" Normal 이므로 자동으로
          글꼴이 바뀐다. 다음 스타일들은 바뀌지 않은 것들이다.
    1. Author
    1. Heading 1, ..., 4 
    1. Title
        1. Subtitle도 바뀐다.
    1. Block Text
1. 화일을 `korean-template.docx`로 새로 저장한다.

