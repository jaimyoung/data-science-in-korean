# R Markdown

## R 안에서의 마크다운 컴파일하기
첫째로 버튼을 눌러 실행하는 방법을 생각해보자.
하지만 한글이 깨어진다.
Rmd 화일의 윗부분에 다음 메타데이타를 추가하면 된다.

```yaml
---
output: 
  word_document:
    highlight: "tango"
    reference_docx: ../intro-practical-data-science/reference.docx
---
```

또다른 방법은 R 세션 안에서 다음을 실행하는 것이다:
```r
render("04_r.Rmd", output_format =
         word_document(reference_docx = "reference_korean_friendly.docx"),
       clean = FALSE)
system("open 04_r.docx")
```
여기서 reference_docx 는 한글을 적당한 폰트로 바꿔준 스타일이어야 한다.

PDF 화일로 출력하는 법은 다음과 같다.
```r
library(rmarkdown)
render("04_r.Rmd", output_format =
         pdf_document(toc=TRUE, latex_engine = 'xelatex',
                      pandoc_args = c("--variable", "mainfont='NanumGothic'")),
       clean = FALSE)
system("open 04_r.pdf")
```
역시 폰트가 골치이다. 

TBA
