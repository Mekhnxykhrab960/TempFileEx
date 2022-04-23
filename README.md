# TempFileEx
#include &lt;File.au3> ; For demonstration purposes only. #include &lt;WinAPIEx.au3>  Example()  Func Example() ; To demonstrate there is very little difference in terms of execution time.     Local $hTimer = TimerInit()     For $i = 1 To 5000         _TempFile()     Next     ConsoleWrite(TimerDiff($hTimer) / 5000 &amp; @CRLF)      $hTimer = TimerInit()     For $i = 1 To 5000         _TempFileEx(Default, Default, 'tmp')     Next     ConsoleWrite(TimerDiff($hTimer) / 5000 &amp; @CRLF)      ConsoleWrite('Example Output: ' &amp; _TempFileEx(@TempDir &amp; '', Default, 'temp.file') &amp; @CRLF) ; The $sFileExtension doesn't have to have the dot (.) at the start of the file extension.     ConsoleWrite('Example Output: ' &amp; _TempFileEx(@TempDir &amp; '', ';', '.....prefix') &amp; @CRLF) ; The $sFileExtension doesn't have to have the dot (.) at the start of the file extension, but in this example there are many! EndFunc   ;==>Example  ; Version: 1.00. AutoIt: V3.3.8.1 ; A different approach to creating a temporary file. See the documentation for _TempFile. Func _TempFileEx($sDirectoryName = @TempDir, $sFilePrefix = '~', $sFileExtension = '.tmp')     If $sDirectoryName = Default Or FileExists($sDirectoryName) = 0 Then         $sDirectoryName = @TempDir     EndIf     If FileExists($sDirectoryName) = 0 Then         $sDirectoryName = @ScriptDir     EndIf     If $sFileExtension = Default Then         $sFileExtension = '.tmp'     EndIf     If $sFilePrefix = Default Then         $sFilePrefix = '~'     EndIf     $sDirectoryName = StringRegExpReplace($sDirectoryName, '+$', '') ; Remove the backslash from the file path - Or _WinAPI_PathRemoveBackslash in WinAPIEx.     $sFileExtension = '.' &amp; StringRegExpReplace($sFileExtension, '^.+', '') ; Remove the initial dot (.) from the file extension.     $sFilePrefix = StringRegExpReplace($sFilePrefix, '[/:*?"&lt;>|]', '') ; Remove any non-supported characters in the file prefix.     Local $sReturn = ''     Do         $sReturn = FileGetShortName($sDirectoryName &amp; '' &amp; $sFilePrefix &amp; StringRegExpReplace(_WinAPI_CreateGUID(), 'V*(w{12})}', '1') &amp; $sFileExtension)     Until FileExists($sReturn) = 0     Return $sReturn EndFunc   ;==>_TempFileEx
