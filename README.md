==The Cocoa Touch XML-RPC Framework==

The Cocoa Touch XML-RPC Framework is a simple, and lightweight, XML-RPC client
framework written in Objective-C for use on iOS devices.
This project was forked off of Eric Czarny's XMLRPC project, which can be located at: https://github.com/eczarny/xmlrpc

Extra code, test client and server, and unit tests have been removed for ease of conversion, but may be re-integrated based on users needs/wants.

===Requirements===

The Cocoa Touch XML-RPC Framework has been built, and designed, for iOS 4.0 or
later.

This version of the Cocoa Touch XML-RPC Framework includes a new event-based XML
parser. The previous tree-based XML parser still exists, but is no longer the
default XML-RPC response parser nor included in the Xcode build. This should
hopefully provide better compatibility with the iPhone SDK.

===Usage===

Invoking an XML-RPC request through the XML-RPC connection manager is easy:

{{{
#!objective-c

NSURL *URL = [NSURL URLWithString: @"http://127.0.0.1:8080/"];	
XMLRPCRequest *request = [[XMLRPCRequest alloc] initWithURL: URL];
XMLRPCConnectionManager *manager = [XMLRPCConnectionManager sharedManager];

[request setMethod: @"Echo.echo" withParameter: @"Hello World!"];

NSLog(@"Request body: %@", [request body]);

[manager spawnConnectionWithXMLRPCRequest: request delegate: self];

[request release];
}}}
-

This spawns a new XML-RPC connection, assigning that connection with a unique
identifier and returning it to the sender. This unique identifier, a UUID
expressed as an NSString, can then be used to obtain the XML-RPC connection from
the XML-RPC connection manager, as long as it is still active.

The XML-RPC connection manager has been designed to ease the management of
active XML-RPC connections. For example, the following method obtains an NSArray
of active XML-RPC connection identifiers:

{{{
#!objective-c

- (NSArray*)activeConnectionIdentifiers;
}}}
-

The NSArray returned by this method contains a list of each active connection
identifier. Provided with a connection identifier, the following method will
return an instance of the requested XML-RPC connection:

{{{
#!objective-c

- (XMLRPCConnection*)connectionForIdentifier:(NSString*)connectionIdentifier;
}}}
-

Finally, for a delegate to receive XML-RPC responses, authentication challenges,
or errors, the XMLRPCConnectionDelegate protocol must be implemented. For
example, the following will handle successful XML-RPC responses:
{{{
#!objective-c

- (void)request:(XMLRPCRequest*)request didReceiveResponse:(XMLRPCResponse*)response
{
    NSLog(@"Response body: %@", [response body]);
}
}}}
-

Refer to XMLRPCConnectionDelegate.h for a full list of methods a delegate must
implement. Each of these delegate methods plays a role in the life of an active
XML-RPC connection.

===Sending synchronous XML-RPC requests===

There are situations where it may be desirable to invoke XML-RPC requests
synchronously in another thread or background process. The following method
declared in XMLRPCConnection.h will invoke an XML-RPC request synchronously:

{{{
#!objective-c

+ (XMLRPCResponse*)sendSynchronousXMLRPCRequest:(XMLRPCRequest*)request error:(NSError**)error;
}}}
-

If there is a problem sending the XML-RPC request expect nil to be returned.

===Acknowledgments===

The origins of this library come from Eric Czarny's XMLRPC code shared on GitHub.

The Base64 encoder/decoder found in NSStringAdditions and NSDataAdditions have
been adapted from code provided by Dave Winer.

The idea for this framework came from examples provided by Brent Simmons, the
creator of NetNewsWire.

===License===

Copyright (c) 2011 Dallas Brown.
Copyright (c) 2010 Eric Czarny.

This library is licensed under the MIT License.

The Cocoa Touch XML-RPC Framework should be accompanied by a LICENSE file, this
file contains the license relevant to this distribution.