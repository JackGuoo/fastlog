# fastlog
Simple, easy and fast log library for Golang

只有壹個文件，可簡單引入項目，速度快輕量級.基於官方log庫做的二次開發

# 與Golang log庫完全兼容,可以直接替換

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
        // 新建壹個logger
        logger := log.New(os.Stderr, "prefix", log.LstdFlags|log.Ldebug)




        // 打印壹條調試日誌 日誌級別 `DEBUG`
        logger.Debug("This is debug msg (Debug)")
        logger.Debugln("This is debug msg (Debugln) ")
        logger.Debugf("This is debug number %d", 2233)

        // 打印壹條壹般日誌 日誌級別 `INFO`
        logger.Info("this is Info msg (Info)")
        logger.Infoln("this is Info msg (Infoln)")
        logger.Infof("%s %d","this is a Info number")

        // 打印壹條警告日誌   日誌級別 `WARN`
        logger.Warn("...")
        logger.Warnln("...")
        logger.Warnf("...")

        // 打印壹條錯誤日誌  日誌級別 `ERROR`
        logger.Error("...")
        logger.Errorln("...")
        logger.Errorf("...")

        // 記錄日誌並拋出壹個panic  日誌級別 `PANIC`
        logger.Panic("...")
        logger.Patnicln("...")
        logger.Patnicf("...")

        // 記錄壹個錯誤日誌並退出整個程序 日誌級別 `ERROR`
        logger.Fatal("...")
        logger.Fataln("...")
        logger.Fatalf("...")
    }



# 修改默認的日誌輸出

日誌默認輸出到`os.Stderr` ,可以使用 SetOutput 修改默認的輸出對象


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


            log.SetFlag(log.Flags()|log.Ldebug)   //設置默認的日誌顯示級別，不設置所有級別的日誌都會被輸出，並且不顯示日誌級別（為了和官方log包保持壹致）

            // 修改默認的日誌輸出對象
            log.SetOutput(outFile)

            log.Info("xxx")
    }


# 日誌級別以及Flag的修改

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

默認情況下，所有級別的輸出都會被打印，並且不顯示日誌的級別（目的是盡量和標準log包保持壹致，如果妳希望顯示日誌級別可通過以下方式設置）

    package main

    import (
            "os"

            log "github.com/go-fastlog/fastlog"
    )

    func main() {
            log.SetFlag(log.Flags()  | log.Linfo)   //設置問默認的日誌個時間格式，日誌級別為info, 低於info級別的日誌如(debug) 將不再被打印
            log.Debug("this is debug msg")  //不會再被打印
            log.Info("xxx")  // 打印 xxx
    }


