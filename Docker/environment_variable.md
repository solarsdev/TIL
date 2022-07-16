# Docker Environment Variable

- 도커를 실행할때 실행환경에서 사용할 변수를 주입해야 하는 경우가 있는데, 컨테이너에 변수를 주입할때 주로 활용되는것이 환경변수임
- 환경변수를 주입하는 방법에는 2가지가 있는데, 직접 환경변수를 입력해서 주입하는 방식과 파일로 전달해주는 방식

## 환경변수 직접 주입하기

```bash
$ docker run -ite MYHOST=solarsdev.com ubuntu:focal bash                                                   ✔  13:14:41
root@42258308e9f5:/# echo $MYHOST
solarsdev.com
root@42258308e9f5:/# env
MYHOST=solarsdev.com
HOSTNAME=42258308e9f5
PWD=/
HOME=/root
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
TERM=xterm
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/env
root@42258308e9f5:/#

$ docker inspect 422
"Env": [
  "MYHOST=solarsdev.com",
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
],
```

## 환경변수 파일로 주입하기

```bash
$ cat sample.env                                                                                           ✔  13:18:01
MY_HOST=solarsdev.com
MY_VAR=123
MY_VAR=456
$ docker run -it --env-file ./sample.env ubuntu:focal env                                                  ✔  13:18:04
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=5b47dad97b7a
TERM=xterm
MY_HOST=solarsdev.com
MY_VAR=456
HOME=/root
```

## Sample: NGINX Image Environment

This behavior can be changed via the following environment variables:

- `NGINX_ENVSUBST_TEMPLATE_DIR`
  - A directory which contains template files (default: `/etc/nginx/templates`)
  - When this directory doesn't exist, this function will do nothing about template processing.
- `NGINX_ENVSUBST_TEMPLATE_SUFFIX`
  - A suffix of template files (default: `.template`)
  - This function only processes the files whose name ends with this suffix.
- `NGINX_ENVSUBST_OUTPUT_DIR`
  - A directory where the result of executing envsubst is output (default: `/etc/nginx/conf.d`)
  - The output filename is the template filename with the suffix removed.
    - ex.) `/etc/nginx/templates/default.conf.template` will be output with the filename `/etc/nginx/conf.d/default.conf`.
  - This directory must be writable by the user running a container.
