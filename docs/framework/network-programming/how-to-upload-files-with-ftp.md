---
title: "How to: Upload Files with FTP"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e40f17c5-dd12-4c62-9dbf-00ab491382dc
caps.latest.revision: 5
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
---
# How to: Upload Files with FTP
This sample shows how to upload a file to an FTP server.  
  
## Example  
  
```csharp  
using System;  
using System.IO;  
using System.Net;  
using System.Text;  
  
namespace Examples.System.Net  
{  
    public class WebRequestGetExample  
    {  
        public static void Main ()  
        {  
            // Get the object used to communicate with the server.  
            FtpWebRequest request = (FtpWebRequest)WebRequest.Create("ftp://www.contoso.com/test.htm");
            request.Method = WebRequestMethods.Ftp.UploadFile;
            // This example assumes the FTP site uses anonymous logon.  
            request.Credentials = new NetworkCredential("anonymous", "janeDoe@contoso.com");

            //Create FileStream so that file is read as it is written to request stream
            using (FileStream sourceStream = new FileStream("testfile.txt", FileMode.Open))
            using (Stream requestStream = request.GetRequestStream())
            {
                // Copy the contents of the file to the request stream.  
                sourceStream.CopyTo(requestStream);
            }
            using (FtpWebResponse response = (FtpWebResponse)request.GetResponse())
            {
                //Respond to StatusCode of response object appropriately
                if (response.StatusCode != FtpStatusCode.CommandOK && response.StatusCode != FtpStatusCode.ClosingData)
                {
                    throw new Exception($"Invalid statusCode, Message: {response.BannerMessage},Status code:{response.StatusCode}");
                }
            }
        }  
    }  
}  
```  
  
## Compiling the Code  
 This example requires:  
  
-   References to the **System.Net** namespace.  
  
## Robust Programming  
  
## .NET Framework Security
