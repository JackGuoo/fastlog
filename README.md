# fastlog
Simple, easy and fast log library for Golang

只有一个文件，可简单引入项目，速度快轻量级.基于官方log库做的二次开发

# 与Golang log库完全兼容,可以直接替换

例子:
    package main

    import (
        log "github.com/go-fastlog/fastlog"
        "os"
    )

    func main() {
        log.Println("this is test string")
        log.Printf("%d this is a test number",22222)
        log.Panic("oh ! panic happen")
    }


# 例子

    package main

    import (
        log "github.com/go-fastlog/fastlog"
        "os"
    )

    func main() {
        // 新建一个logger
        logger := log.New(os.Stderr, "prefix", log.LstdFlags|log.Ldebug)




        // 打印一条调试日志 日志级别 `DEBUG`
        logger.Debug("This is debug msg (Debug)")
        logger.Debugln("This is debug msg (Debugln) ")
        logger.Debugf("This is debug number %d", 2233)

        // 打印一条一般日志 日志级别 `INFO`
        logger.Info("this is Info msg (Info)")
        logger.Infoln("this is Info msg (Infoln)")
        logger.Infof("%s %d","this is a Info number")

        // 打印一条警告日志   日志级别 `WARN`
        logger.Warn("...")
        logger.Warnln("...")
        logger.Warnf("...")

        // 打印一条错误日志  日志级别 `ERROR`
        logger.Error("...")
        logger.Errorln("...")
        logger.Errorf("...")

        // 记录日志并抛出一个panic  日志级别 `PANIC` 
        logger.Panic("...")
        logger.Patnicln("...")
        logger.Patnicf("...")

        // 记录一个错误日志并退出整个程序 日志级别 `ERROR`
        logger.Fatal("...")
        logger.Fataln("...")
        logger.Fatalf("...")
    }



# 修改默认的日志输出

日志默认输出到`os.Stderr` ,可以使用 SetOutput 修改默认的输出对象


    package main

    import (
            "os"

            log "github.com/go-fastlog/fastlog"
    )

    func main() {
            outFile, err := os.OpenFile("log.txt", os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0644)
            if err != nil {
                    log.Fatal(err.Error())
            }


            log.SetFlag(log.Flags()|log.Ldebug)   //设置默认的日志显示级别，不设置所有级别的日志都会被输出，并且不显示日志级别（为了和官方log包保持一致）

            // 修改默认的日志输出对象
            log.SetOutput(outFile)

            log.Info("xxx")
    }


# 日志级别以及Flag的修改

    const (
        Ldate         = 1 << iota // the date in the local time zone: 2009/01/23  
        Ltime                     // the time in the local time zone: 01:23:23
        Lmicroseconds             // microsecond resolution: 01:23:23.123123.  assumes Ltime.
        Llongfile                 // full file name and line number: /a/b/c/d.go:23
        Lshortfile                // final file name element and line number: d.go:23. overrides Llongfile
        LUTC                      // if Ldate or Ltime is set, use UTC rather than the local time zone
        Ldebug                    // debug level
        Linfo                     // info level
        Lwarn                     // Warn level
        Lerror                    // Error level
        Lpanic                    // panic level
        LstdFlags = Ldate | Ltime | Llongfile // initial values for the standard logger

    )

默认情况下，所有级别的输出都会被打印，并且不显示日志的级别（目的是尽量和标准log包保持一致，如果你希望显示日志级别可通过以下方式设置）

    package main

    import (
            "os"

            log "github.com/go-fastlog/fastlog"
    )

    func main() {
            log.SetFlag(log.Flags()  | log.Linfo)   //设置问默认的日志个时间格式，日志级别为info, 低于info级别的日志如(debug) 将不再被打印
            log.Debug("this is debug msg")  //不会再被打印
            log.Info("xxx")  // 打印 xxx 
    }

