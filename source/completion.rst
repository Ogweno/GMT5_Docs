命令行补全
==========

GMT为bash提供了命令行补全功能，在命令行中敲GMT命令时，可以使用tab键补全部分选项。不过补全功能很弱，不用也罢。

#. 安装 ``bash-completion``
#. 在 ``~/.bashrc`` 中加入如下语句

   .. code-block:: bash

      # Use bash-completion, if available
      if [ -r /usr/share/bash-completion/bash_completion ]; then
          . /usr/share/bash-completion/bash_completion
      fi

      . ${GMTHOME}/share/tools/gmt_completion.bash

#. 执行 ``source ~/.bashrc`` 使修改生效
#. 在命令行键入即可看到补全效果::

       $ gmt psxy -[Tab]
       -^  -?  -A  -B  -C  -E  -g  -h  -I  -K  -N  -p  -R  -S  -T  -V  -X
       -:  -a  -b  -c  -D  -f  -G  -i  -J  -L  -O  -P  -s  -t  -U  -W  -Y
