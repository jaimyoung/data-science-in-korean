# 윈도우즈에서 (EUC-KR) 한글

윈도즈 시스템에서 작성한 CSV화일을 OSX에서 열려고 하면 
한글이 깨져있을 때가 있다.
원본화일이 euc-kr 인코딩이어서 그렇다.
현재 대세는 utf-8이다 (이문서[^1]와 이문서[^2]참조).

보통 터미널에서 다음을 실행하면 해결된다[^3].

    iconv -f euc-kr -t utf-8 old.csv > new.csv

[^1]: <https://en.wikipedia.org/wiki/Extended_Unix_Code#EUC-KR>
[^2]: <http://studyforus.tistory.com/167>
[^3]: <https://benant.wordpress.com/2012/04/03/euc-kr-인코딩-파일을-utf-8으로-변환하는-방법에-대한-글들-정/>
