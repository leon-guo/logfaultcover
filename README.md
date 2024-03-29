背景：
 异常测试是软件测试中非常重要的一环，异常测试充分与否直接影响到测试质量和产品线上稳定性。
logcover是一款专门用于度量异常测试覆盖率的轻量级工具，通过度量异常日志的测试覆盖率来反应异常测试覆盖率，因为打印异常日志的程序分支往往更需要测试覆盖的异常分支。


原理：
 结合程序源代码和测试执行产生的日志，计算程序异常日志的测试覆盖率。
例如： 源代码中有100处异常日志(warning、fatal、error)打印点，程序在测试过程中，产生了100条日志，对应源代码中50处日志打印点，则异常日志覆盖率是50%，同时，logcover会给出覆盖日志和未覆盖日志的所有信息，包括文件名、行号等，便于快速识别未覆盖异常日志。


实现：
• 根据svn，对源代码进行轻量级静态分析，获得代码中异常日志打印的原始信息
• 收集单机/多机测试日志，并对日志文件进行parse、filter、merge等处理，得到实际覆盖的日志信息
• 根据代码中的原始日志信息和实际测试产生的日志信息，做diff计算，得出异常log覆盖率和覆盖信息
• 覆盖率报告邮件推送


logcover使用说明:
1: 下载logcover
2: 修改对应 logcover.cfg文件 
   logcover_type =0 表示单机模式；logcover_type=1表示多机模式。
   选择多机模式时，对应填写 machines、user、password、log_paths和script_path 
3: 执行 sh logcover.sh $svn $log_cover_log_dir $mail_list -s $mail_subject 
   $svn : 被测程序svn源码路径
   $log_cover_log_dir: log文件存放路径
   $mail_list : 覆盖率报告推送邮件列表，多邮件中间以空格分隔。例如:zhangsan@xx.com li@xx.com
   -s $mail_subject : 推送邮件自定义主题

logcover执行环境依赖:
1: perl v5.8.5+ 
2: python v2.7+
3: svn client v1.6.5+
