# ƏMƏLİYYAT SİSTEMLƏRİNDƏ STANDARD İNPUT VƏ OUTPUT

Əməliyyat sistemlərində istifadəçidən məlumat almaq və istifadəçiyə məlumat göstərmək üçün virtual tunellər nəzərdə tutulub. İstifadəçidən məlumat almaq üçün tunel **standard input** (bundan sonra STDIN), məlumatı çıxartmaq üçün isə **standard output** (bundan sonra STDOUT) nəzərdə tutulub. Hər bir əməliyyat sistemində default olaraq standard input klaviaturaya bağlıdır, standard output isə kompüterin monitoruna bağlıdır. Yəni biz klaviaturada yazı yazarkən həmən informasiyanı əməliyyat sistemi STDIN tuneli vasitəsi ilə qəbul edir, STDOUT tuneli vasitəsi ilə isə monitorda bizə göstərir. Lakin əməliyyat sistemləri bu standard input/output mənbələrini dəyişməyə imkan verir. Misalçün məlumat daxil etmək üçün mənbə olaraq biz "tunelin yönünü" klaviaturadan hər hansı bir fayla yönləndirə bilərik, bunun vasitəsilə də bu biz məlumatı klaviaturadan yox faylın içərisində saxlanan datadan əldə edirik. Və ya hər hansısa bir proqram işlədiyi zaman STDOUT-a yönləndirdiyi məlumatı biz monitor əvəzinə hər hansı bir fayla yönləndirə bilərik. Bu zaman məlumat ekrana çap olunmaq əvəzinə fayla yazılacaq. Bunlar haqda nümunələr birazdan aşağıda göstəriləcək. Lakin gəlin əvvəlcə standard input/output növləri ilə ətraflı tanış olaq.

## İnput & output növləri

İnput və outputlar təyinatlarına görə bir neçə qrupa bölünür. Daha doğrusu inputun yalnızca bir növü vardır və bu hər hansı bir informasiya ola bilər (binary və ya text). Lakin output-un iki növü vardır: normal İnformasiya xarakterli məlumatlar (STDOUT) və ya xəta informasiya xarakterli məlumatlar (STDER). Output-un iki növünün olma səbəbi istifadəçiyə təqdim olunan məlumatları xarakterinə görə qruplaşdırmaqdır ki, informasiyanın təyinatı bəlli olsun. Misalçün UNİX əməliyyat sistemində hər hansı bir qovluğun içərisindəki faylların siyahısını görmək üçün komanda aşağıdakı kimidir:

```bash
ls -la /existing_category/
```

Yuxarıdakı komandanın nəticəsi əgər ***existing_category*** qovluğu mövcuddursa onda onun daxilindəki bütün fayl və qovluqlar haqda mətn şəklində informasiya olacaq və biz monitorda təxmini aşağıdakı kimi mətn görəcəyik:

```
total 14997676
drwxr-xr-x  38 user1 user1      12288 Dec  3 19:04  .
drwxr-xrwx 100 user1 user1      16384 Dec 11 23:24  ..
-rw-r--r--   1 user1 user1     109826 Oct  2  2018 'myfile.txt'
drwxrwxr-x   4 user1 user1       4096 Apr  4  2019  myfolder
-rw-rw-r--   1 user1 user1    3835895 May 19  2019 'mypicture.jpg'
-rw-rw-r--   1 user1 user1    3835895 May 19  2019 'myvideo.mp4'
```

Bu informasiyanı əməliyyat sistemi ***STDOUT*** tuneli vasitəsi ilə bizə göstərdi. Lakin ***ls -la*** komandasına içərisinə baxmaq üçün mövcud olmayan kateqoriya adı versəydik onda bu komanda məntiqli olaraq bizə problem haqda xəta mesajı verməlidir ki, nə baş verdiyindən xəbərimiz olsun. Əgər bu informasiya eyni tunel vasitəsi ilə bizə ötürsəydi (STDOUT) bu mesajın xəta və ya normal informasiya olduğunu necə təyin edəcəkdik? Bu problemi aradan qaldırmaq üçün output tuneli STDOUT (normal informasiyalar üçün) və STDER (xəta xarakterli informasiyalar üçün) qruplarına bölünüb.

Gəlin indi input və output tunellərinin yönünü necə dəyişməyə nəzər salaq.

## PIPE və REDIRECT

Tunellərin yönünü bir mənbədən digərinə dəyişmək üçün ```|``` (pipe) və ```<``` və ya ```>``` və ya ```>>``` (redirect) işarələrindən istifadə olunur.

### PIPE
```|``` işarəsi ilə biz bir komandanın nəticəsini (output) digər komandaya giriş (input) kimi ötürə bilərik. Misalçün ```ls -la``` komandasının nəticəsini ```grep``` komandasına ötürərək ```grep``` vasitəsi ilə nəticələrin içərisindən ancaq lazım olanları filterləyib görə bilərik. Təsəvvür edin ```existing_folder``` qovluğunun içərisində yüzlərlə fayl vardır. Lakin biz yalnızca adının sonu .jpg ilə bitən faylları görmək istəyirik:

```bash
ls -la /existing_folder/ | grep .jpg
```

Nəticə yüzlərlə fayl əvəzinə yalnızca adında .jpg sözü keçən fayllar olacaq.

### REDIRECT

#### STDIN tunelini yönləndirmək

Gəlin **Python** dilində istifadəçidən terminalda adını daxil etməsini istəyərək onu adıyla salamlayan sadə bir skript yazaq:

```python
name = input("Adınızı daxil edin: ")

print(name + ", Siz xoş gəlmişsiniz!")
```

Bu skripti terminalda **Python** vasitəsi ilə işlətsək aşağıdakılar baş verəcək:

```bash
>> python script.py
>> Adınızı daxil edin: 
...
>> Adınızı daxil edin: Gülnaz
>> Gülnaz, Siz xoş gəlmişsiniz!
```

Amma biz Gülnaz adını terminaldan klaviatura vasitəsilə deyil hər hansı bir fayla yazaraq proqramın o adı həmən fayldan götürməsini istəyiriksə ```<``` işarəsindən istifadə etməliyik:

```bash
>> python script.py < ad_olan_fayl.txt
>> Adınızı daxil edin: Gülnaz, Siz xoş gəlmişsiniz!
```

#### STDOUT tunelini yönləndirmək

Əvvəlki nümunələrdə ```ls -la``` komandasının qovluğun daxilindəki fayllar və digər qovluqlar haqda informasiyanı mətn şəklində çıxardığını göstərmişdik. Lakin bu dəfə biz bu komandanın nəticəsinin bir başa monitora yox adi fayla mətn şəklində yazılmasını istəyirik. Bu zaman aşağıdakı nümunədə göstərildiyi kimi etməliyik.

```bash
ls -la /existing_folder/ > my_file.log
```

Komandanın işi bitdikdən sonra biz ekranda heç nə görməyəcəyik, lakin **my_file.log** faylını açıb baxdığımız zaman ekranda görməli olduğumuz yazının həmən faylın içərisinə yazıldığını görəcəyik. ```>``` işarəsi vasitəsi ilə STDOUT tunelinin yönünü ekrandan fayla dəyişdik. 

Gəlin ```ls -la``` komandasını başqa bir qovluq üçün işlədək və nəticəni yenə də əvvəlki **my_file.log** faylına yazmağa çalışaq:

```bash
ls -la /another_existing_folder/ > my_file.log
```

Əgər ***my_file.log*** faylının içərisinə baxsanız onda əvvəlki məlumatların silindiyini və yeni məlumatların yazıldığını görəcəyik. Bəs əgər biz əvvəlki məlumatları itirmək istəmirdiksə və eyni zamanda yeni məlumatların da həmən fayla ardıcıl yazılmasını istəyirdiksə? Onda köməyə ```>>``` işarəsi çatır. Bu işarə özündən əvvəlki ilə eyni işi görür, fərqi isə yönləndirildiyi faylda köhnə məlumatları qoruyaraq yeni məlumatları faylın sonuna əlavə etməsidir:

```bash
ls -la /another_existing_folder/ >> my_file.log
```

### STDOUT və STDER

Yuxarıdakı nümunələrdə biz gördük ki, ```>``` və ```>>``` işarələri vasitəsi ilə biz nəticələri monitordan fayla yönəldə bilirik. Lakin bu işarələr default olaraq yalnızca məlumat xarakterli informasiyaları fayla yönəldir. Əgər ```ls -la``` komandasında xəta baş versəydi bu informasiya fayla yazılmaq əvəzinə ekranda görünəcəkdi, baxmayaraq ki, biz redirect istifadə etmişik:

```bash
ls -la /fake/folder/ > my_file.log
```

Yuxarıdakı komandanın nəticəsini monitorda belə görəcəyik:

```
ls: cannot access '/fake/folder/': No such file or directory
```

Redirect vasitəsi ilə fayla yazacağımız məlumat növünü fərqləndirmək üçün bu məlumat növlərinin rəqəmini də əlavə etməliydik. Uyğun olaraq STDİN, STDOUT və STDER üçün 0, 1 və 2 rəqəmləri nəzərdə tutulub. Gəlin yuxarıdakı komandanı biraz fəqrli yazaq:

```bash
ls -la /fake/folder/ 2> my_file.log
```

Gördüyünüz kimi ```>``` işarəsinin qarşısına 2 rəqəmi yazaraq əməliyyat sisteminə yalnızca xəta xarakterli mesajların (STDER tunelinə gələn məlumatın) **my_file.log** faylına yazılmasını bildirdik. Xəta mesajı bu dəfə ekranda yox fayla yazılacaq. Bəs komanda xəta çıxarmasa necə (yəni informasiya STDOUT tunelinə gəlsə)? Bu zaman isə əksinə, xəta mesajı fayla yazıldığı halda normal mesaj ekranda çap olunacaq. Bəs hər iki növ tuneldən gələn məlumatların hər ikisni də (həm STDOUT həm də STDER) fayla yazmaq istəsək necə? Bu zaman aşağıdakı kimi etməliyik:

```bash
ls -la /fake/folder/ > my_file.log 2>&1
```

Və ya

```bash
ls -la /fake/folder/ &> my_file.log
```

Bu zaman hər iki növ informasiya fayla yazılacaq.

Əgər biz tunellərdən gələn məlumatların ümumiyyətlə heç bir yerə yazılmasını istəmiriksə onda UNİX/LİNUX əməliyyat sistemlərində xüsusi **/dev/null** faylı nəzərdə tutulmuşdur ki, bu da həmən fayla yönləndirilən istənilən informasiyanı terminate edir (sadəcə heç bir yerə yazmır).

```bash
ls -la /any_folder/ &> /dev/null
```