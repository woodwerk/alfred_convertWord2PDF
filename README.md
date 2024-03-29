# Convert Word to PDF
A file-action triggered workflow that converts the selected Word document to a PDF.

![](https://raw.githubusercontent.com/woodwerk/alfred_convertWord2PDF/master/convertWord2PDF.png)

The selected file is input to an AppleScript that does the heavy lifting.

```

on run argv
	repeat with i from 1 to count of argv
		set input to POSIX file (item i of argv) as alias
		
		set output to {}
		tell application "Microsoft Word" to set theOldDefaultPath to get default file path file path type documents path
		try
			set theDoc to contents of input
			tell application "Finder"
				set theFilePath to container of theDoc as text
				set ext to name extension of theDoc
				set theName to name of theDoc
				
				copy length of theName to l
				copy length of ext to exl
				
				set n to l - exl - 1
				copy characters 1 through n of theName as string to theFilename
				
				set theFilename to theFilename & ".pdf"
				
				tell application "Microsoft Word"
					set default file path file path type documents path path theFilePath
					open theDoc
					set theActiveDoc to the active document
					save as theActiveDoc file format format PDF file name theFilename
					copy (POSIX path of (theFilePath & theFilename as string)) to end of output
					close theActiveDoc
				end tell
			end tell
		end try
	end repeat
	
	tell application "Microsoft Word" to set default file path file path type documents path path theOldDefaultPath
	return output
	
end run

```
