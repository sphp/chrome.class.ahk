# chrome.class.ahk

```
#SingleInstance, Force
#Include %A_ScriptDir%\chclass.ahk
SetWorkingDir, %A_ScriptDir%
chport := "52222"
pageid := "09FE8F0539AB6DDF6302E3C4B71AAA95"
inifil := Script() . ".ini"

;inst := new Chrome(,,,,chport)
;inst.page.Evaluate("window.location.href = 'https://google.com'")

Loop{
	if fileexist(inifil) {
		IniRead, chport, % inifil, app, chport
		IniRead, pageid, % inifil, app, pageid
	}
	if pageid && chport {
		if(inst := chrome.GetBy({id:pageid, port:chport})){
			thisurl := inst.Evaluate("window.location.href").value
			if instr(thisurl,"google.com") {
				inst.Call("Page.navigate", {"url": "https://search.yahoo.com/"})
				inst.WaitForLoad()
			}
			else if instr(thisurl,"search.yahoo.com") {
				inst.Evaluate(script)
				inst.Call("Page.navigate", {"url": "https://bing.com"})
				inst.WaitForLoad()
			}
			else if instr(thisurl,"bing.com") {
				inst.Evaluate(script)
				inst.Call("Page.navigate", {"url": "https://google.com"})
				inst.WaitForLoad()
			}
		}else{
			inst := new Chrome(,,,,chport)	;MsgBox % inst.pageid "`n" inst.pagews
			inst.page.Call("Page.navigate", {"url": "https://google.com"})
			inst.page.WaitForLoad()
			inst.page.Call("Page.navigate", {"url": "https://search.yahoo.com/"})
			inst.page.WaitForLoad()
			IniWrite, % inst.pageid, % inifil, app, pageid
		}
	}
}
Esc::ExitApp
Script(){
	return StrReplace(StrReplace(A_ScriptName,".ahk"),".exe")
}
```
