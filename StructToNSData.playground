//: Playground - noun: a place where people can play

import UIKit

enum MessageType {
    case kMessageTypeRandomNumber, kMessageTypeRandomWords, kMessageTypeGameBegin, kMessageTypeWordFound, kMessageTypeNewWordToFind, kMessageTypeGameOver
}

struct Message {
    let message : MessageType
}

struct MessageRandomWords {
    let message = MessageType.kMessageTypeRandomWords
    var data : NSData?
    var name: String
    
    struct ArchivedPacket {
        let message = MessageType.kMessageTypeRandomWords
        var dataLength : Int64
        var nameLength : Int64
    }
    
    func archive() -> NSData {
        var archivedPack = ArchivedPacket(dataLength: Int64(self.data!.length), nameLength: Int64(self.name.lengthOfBytesUsingEncoding(NSUTF8StringEncoding)))
        var metaData = NSData(bytes: &archivedPack, length: sizeof(ArchivedPacket))
        let archiveData = NSMutableData(data: metaData)
         archiveData.appendData(name.dataUsingEncoding(NSUTF8StringEncoding, allowLossyConversion: false)!)
        archiveData.appendData(data!)
        return archiveData
    }

    static func unarchive(data : NSData!) -> MessageRandomWords {
        var archivedPacket = ArchivedPacket(dataLength: 0, nameLength: 0)
        let archivedStructLength = sizeof(ArchivedPacket) //lenght of the struct
        
        //Get the data tha will form our archived Packet
        let archivedData = data.subdataWithRange(NSMakeRange(0, archivedStructLength))
        //save the data taht form the archivedPacket inside the archivedPacket
        archivedData.getBytes(&archivedPacket, length: archivedStructLength)
        //get the range of data that contains the name
        let nameRange = NSMakeRange(archivedStructLength, Int(archivedPacket.nameLength))
        //get the range of the data that contains the data
        let dataRange = NSMakeRange(archivedStructLength + Int(archivedPacket.nameLength), Int(archivedPacket.dataLength))
        //get the data that rappresent the name
        let nameData = data.subdataWithRange(nameRange)
        //Get the name frome the data
        let name = NSString(data: nameData, encoding: NSUTF8StringEncoding) as! String
        // Geth the data
        let theData = data.subdataWithRange(dataRange)
        
        //Create the struct
        let messageRndm = MessageRandomWords(data: theData, name: name)
        return messageRndm
    }
    
}


let testArrayToSend = ["Hello","Test", "Another test word"]
let arrayData = NSKeyedArchiver.archivedDataWithRootObject(testArrayToSend)


let msgRnd = MessageRandomWords(data: arrayData, name: "String to send")
let msgRndData = msgRnd.archive()
msgRndData.length


var message2 : Message?
msgRndData.getBytes(&message2, length: sizeof(Message))

if message2?.message == MessageType.kMessageTypeRandomWords {
    let messageRnd = MessageRandomWords.unarchive(msgRndData)
    messageRnd.name
    let arrayOfWords = NSKeyedUnarchiver.unarchiveObjectWithData(messageRnd.data!) as! [String]

}
