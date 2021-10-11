' Import polygons

Sub Main ()

Dim filePath As String
Dim fileName As String
Dim fileContents As String
Dim count As Integer
Dim StringArr() As String
Dim loopCounter As Integer
filePath = "C:\Users\objoerkqvist\OneDrive - ETH Zurich\Documents\MATLAB\Coil design\polygon_export\"
fileName = filePath + "prototype1_v3_polygon2.txt"

fileContents = ReadDataFromFile(fileName)
count=CInt(UBound(Split(fileContents)))
ReDim StringArr(count)
StringArr = Split(fileContents)

loopCounter = 2
With Polygon
	.Reset
	.Name "Polygon1"
	.Curve "curve1"
	.Point CStr(CDbl(StringArr(0))), CStr(CDbl(StringArr(1)))
	While(loopCounter < (count + 1)/2)
     .LineTo CStr(CDbl(StringArr(loopCounter))), CStr(CDbl(StringArr(loopCounter+1)))
     loopCounter = loopCounter + 2
     Wend
	.Create
End With




End Sub

Function ReadDataFromFile(DataFileName As String) As String
	Dim DataFile As Long, DataStringLength As Long
	DataFile = FreeFile()
	Open DataFileName For Input As #DataFile
	ReadDataFromFile = Input(LOF(DataFile), DataFile)
	Close DataFile
	' Replace different line breaks with vbNewLine, order is important
	ReadDataFromFile = Replace(ReadDataFromFile, vbCrLf, vbLf)
	ReadDataFromFile = Replace(ReadDataFromFile, vbLf, vbCr)
	ReadDataFromFile = Replace(ReadDataFromFile, vbCr, vbNewLine)
	' In case of multiple line breaks: Remove double line breaks
	DataStringLength = -1
	While (Len(ReadDataFromFile)<>DataStringLength)
		DataStringLength = Len(ReadDataFromFile)
		ReadDataFromFile = Replace(ReadDataFromFile, vbNewLine+vbNewLine, vbNewLine) ' remove double line breaks
		ReadDataFromFile = Replace(ReadDataFromFile, "  ", " ") ' remove multiple spaces
	Wend

End Function

