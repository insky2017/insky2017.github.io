
	
## Display CRLF as ^M:
:e ++ff=unix

## Substitute CRLF for LF:
:setlocal ff=unix
:w
:e
