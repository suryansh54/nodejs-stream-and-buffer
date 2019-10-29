# Stream and Buffer Concepts in Node.js


## Stream
Streams are objects that lets you read data from a source or write data to the destination in a continuous fashion.

**Problems with Large Data:**
- **Speed:** Too slow because it has to load all the requests.
- **Buffer Limit:** 1 GB

**Stream Benefits:**
- Abstraction for continuous chunking of data.
- No need to wait for the entire resource to load.

**Stream is Used In:**
- HTTP request & responses
- Standard input/output(stdin & stdout)
- File reads and write

**Types of Stream:**
- Readable Stream
- Writable Stream
- Duplex Stream 

#### 1.Readable Stream
- Readable Stream is used for read operations.
- Standard input streams has data going into the applications.This is achieved through the read operation. Input typically comes from the keyboard used to start the process.

```javascript
const fs = require('fs');
let data = '';

// Create a readable stream
let readableStream = fs.createReadStream('input.txt');

// Set the encoding to be utf8. 
readerStream.setEncoding('UTF8');

// Handle stream events --> data, end,
readableStream.on('data', function(chunk) {
   data += chunk;
});

readableStream.on('end', function(){
   console.log(data);
});
```

#### 2.Writable Stream
- Writable Stream is used for write operations.
- Standard output streams contain data going out of the applications.To write to stdout, we use the write function.

```javascript
process.stdout.write('A Simple Message \n');
```

#### 3.Duplex Stream
- This is a Stream which can be used for both read and write operations.

#### 4.Transfer Stream
- A type of duplex stream where the output is computed based on input.

**What is Piping in a Stream?**
Piping is a process in which we provide the output of one stream as the input to another stream. It is normally used to get data from one stream and to pass the output of that stream to another stream. There is no limit on piping operations.

```javascript
const fs = require('fs');

// Create a readable stream
let readableStream = fs.createReadStream('input.txt');

// Create a writable stream
let writeableStream = fs.createWriteStream('output.txt');

// Pipe the read and write operations
// read input.txt and write data to output.txt
 readerStream.pipe(writerStream);

console.log('End of the Process');
```

## Buffer
Node.js provides Buffer class which provides instances to store raw data.

```javascript
//create an uninitiated Buffer of 10 octets
let bufferOne = new Buffer(10);
//create a Buffer from a given array
let bufferTwo = new Buffer([10, 20, 30, 40, 50]);
//create a Buffer from a given string
let bufferThree = new Buffer('Simply Easy Learning');
```

```javascript
let buffer = Buffer.alloc(26);
for(let i=0; i<26; i++){
    buffer[i]=i+97;
}
console.log(buffer.toString('utf8'));// a, b, c.....z
```

- Instances of the Buffer class are similar to arrays of integers from 0 to 255 (other integers are coerced to this range by & 255 operation) but correspond to fixed-sized, raw memory allocations outside the V8 heap. The size of the Buffer is established when it is created and cannot be changed.
- The Buffer class is within the global scope, making it unlikely that one would need to ever use require('buffer').Buffer.

```javascript
// Creates a zero-filled Buffer of length 10.
const buf1 = Buffer.alloc(10);

// Creates a Buffer of length 10, filled with 0x1.
const buf2 = Buffer.alloc(10, 1);

// Creates an uninitialized buffer of length 10.
// This is faster than calling Buffer.alloc() but the returned
// Buffer instance might contain old data that needs to be
// overwritten using either fill() or write().
const buf3 = Buffer.allocUnsafe(10);

// Creates a Buffer containing [0x1, 0x2, 0x3].
const buf4 = Buffer.from([1, 2, 3]);

// Creates a Buffer containing UTF-8 bytes [0x74, 0xc3, 0xa9, 0x73, 0x74].
const buf5 = Buffer.from('tést');

// Creates a Buffer containing Latin-1 bytes [0x74, 0xe9, 0x73, 0x74].
const buf6 = Buffer.from('tést', 'latin1');
```

##### Buffer.from(), Buffer.alloc(), and Buffer.allocUnsafe()

- **Buffer.from(array)** returns a new Buffer that contains a copy of the provided octets.
- **Buffer.from(arrayBuffer[, byteOffset[, length]])** returns a new Buffer that shares the same allocated memory as the given ArrayBuffer.
- **Buffer.from(buffer)** returns a new Buffer that contains a copy of the contents of the given Buffer.
- **Buffer.from(string[, encoding])** returns a new Buffer that contains a copy of the provided string.
- **Buffer.alloc(size[, fill[, encoding]])** returns a new initialized Buffer of the specified size. This method is slower than **Buffer.allocUnsafe(size)** but guarantees that newly created Buffer instances never contain old data that is potentially sensitive. A TypeError will be thrown if size is not a number.
- **Buffer.allocUnsafe(size)** and **Buffer.allocUnsafeSlow(size)** each return a new uninitialized Buffer of the specified size. Because the Buffer is uninitialized, the allocated segment of memory might contain old data that is potentially sensitive.
