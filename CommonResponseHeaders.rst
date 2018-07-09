***************************
Common Response Headers
***************************

The following describes some of the common response lines returned by HDF Kita.

 * Status Line: the first line of the response will always be: "``HTTP/1.1``" followed by 
    a status code (e.g. 200) followed by a reason message (e.g. "``OK``").  For errors, 
    an additional error message may be included on this line.

 * Content-Length: the response size in bytes.

 * Etag: a hash code that indicates the state of the requested resource.  If the client
    sees the same Etag value for the same request, it can assume the resource has not           
    changed since the last request.

 * Content-Type: the mime type of the response.

