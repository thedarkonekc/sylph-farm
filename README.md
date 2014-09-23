sylph-farm
==========





#Region ;**** Directives created by AutoIt3Wrapper_GUI ****
#AutoIt3Wrapper_Compile_Both=y
#EndRegion ;**** Directives created by AutoIt3Wrapper_GUI ****
; Drücke Esc um das Script zu beenden, Pause um es zu pausieren

Global $Paused, $vab, $countdown = 0
HotKeySet("p", "TogglePause")
HotKeySet("{ESC}", "Terminate")
HotKeySet("+!d", "ShowMessage") ;Shift-Alt-d

;;;; Hier ist der Hauptteil des Programms ;;;;
;$Variabel = InputBox("Login", "Username:")
;$Variabe2 = InputBox("Login", "Passwort:")
;MsgBox(0, "Login", "Hallo " & $Variabel & " Sie haben sich erfolgreich eingeloggt!")
$size_y= 900
$size_x= 1500

$abstandx = @DesktopWidth-$size_x
$abstandy = @DesktopHeight-$size_y
;~ 75 x 90

$mob_x = (65 + ($abstandx/2))
$mob_y = (84 + ($abstandy/2))

$mob_x2 = (1323 + ($abstandx/2))

$mob_y2 = (776 + ($abstandy/2))

$cccount = 0

Func search_Mob()
$varc = 0
$search = PixelSearch( $mob_x, $mob_y, $mob_x2, $mob_y2, 0xFF0004 )
$cccount = $cccount + 1

If IsArray($search) Then
$cccount = 0
ToolTip("")
MouseMove($search[0]+5, $search[1]+60, 1)
$search2 = PixelSearch( $search[0], $search[1], $search[0]+100, $search[1]+100, 0xD8D0C8 )
If IsArray($search2) Then
MouseClick("left")
sleep(2000)
$varc=1
EndIf
ConsoleWrite("Sucht Pixel")
;~ nicht ein monster angreifen was bereits angegriffen wird

$search_loot = PixelSearch( 556 + ($abstandx/2), 685 + ($abstandy/2), 638 + ($abstandx/2) , 728 + ($abstandy/2), 0x030303 )
If IsArray($search_loot) Then
ConsoleWrite('Monster angeclickt')
sleep(5000)
$vab = "kampf"
Else
if($varc == 1) Then
$sToolTip = ToolTip("BLOCK " & @CRLF & "BLOCK" & @CRLF & "BLOCK", $search[0]-20, $search[1]-20, 'Copyright by Vegeta17')
MouseMove(400, 400, 1)
WinSetOnTop('hallo','',1)
EndIf
EndIf
Else
;~ Portal()
;~ sleep(1000)
ConsoleWrite($varc)

EndIf
if($cccount > 100) Then
MouseMove(1498-90+($abstandx/2), 190-75+($abstandy/2), 1)
MouseClick("left")
sleep(500)
MouseMove(529-90+($abstandx/2), 700-75+($abstandy/2), 1)
MouseClick("left")
sleep(500)
MouseMove(488-90+($abstandx/2), 159-75+($abstandy/2), 1)
MouseClick("left")
$cccount=0
Endif

Return $vab;

EndFunc


Func kampf()
;~ Kampf funktion
ConsoleWrite('Lädt Kampf')
$countdown = $countdown + 1
$search_kampf = PixelSearch( 515+($abstandx/2), 785+($abstandy/2), 980+($abstandx/2), 845+($abstandy/2), 0x080807 )
If IsArray($search_kampf) Then
Send("{1}")
Send("{2}")
Send("{up}")
Send("{down}")
Send("{left}")
Send("{right}")
$end_kampf = PixelSearch( 113+($abstandx/2), 165-75+($abstandy/2), 362-90+($abstandx/2), 177-75+($abstandy/2), 0x800303 )
If IsArray($end_kampf) Then
$vab = false
$countdown = 0
Endif


Elseif $countdown > 5 Then
;~ nicht kampf
$vab = false;
$countdown = 0

ConsoleWrite('Nicht im kampf');
EndIf
Return $vab;
EndFunc
Func Portal()
;~ Wenn ein ausgang gesehen wird in gegengestzter richtung drücken
$search = PixelSearch( 415-90+($abstandx/2), 180-75+($abstandy/2), 1350-90+($abstandx/2), 918-75+($abstandy/2), 0x070906 )
If IsArray($search) Then
$movey=460
$movex=860
if($search[0] < 700) Then
$movex = 1000
Else
$movex = 600
EndIf

if($search[1] < 300) Then
$movey = 650
Else
$movex = 350
EndIf


MouseMove($movex, $movey, 1)
MouseClick("left")
Else
MouseClick("left",Random(439,1309,1),Random(273,76 0,1))
Sleep(1000)
EndIf
EndFunc
Sleep(5000)
While 1
Sleep(300)
If $vab == "kampf" Then
$vab = kampf();
Else
$vab = search_Mob()
EndIf
WEnd
;;;;;;;;

Func TogglePause()
$Paused = NOT $Paused
While $Paused
sleep(100)
ToolTip('Script ist pausiert',0,0)

WEnd
ToolTip("")
EndFunc

Func Terminate()
Exit 0
EndFunc

Func ShowMessage()
MsgBox(4096,"","Das ist eine Nachricht.")
EndFunc
