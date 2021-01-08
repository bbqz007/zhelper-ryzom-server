[DOC LICENSE](https://github.com/bbqz007/zhelper-ryzom-server/LICENSE)
* [**IStream**](#IStream)
	- [header](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/nel/include/nel/misc/stream.h)
	- mode: input: false or true, 决定serial的行为，反序列化还是序列化。
	- 非纯接口抽象类，[inline](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/nel/include/nel/misc/stream_inline.h)定义了序列化反序列化算法骨架，必须依赖继承类实现serailBuffer函数。
* [**CMemStream**](#CMemStream)
    - [CObjectVector](#CObjectVector)
    - [CMemStreamBuffer](#CMemStreamBuffer)
    - [header](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/nel/include/nel/misc/mem_stream.h)
* [**CBitMemStream**](#CBitMemStream)
	- [header](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/nel/include/nel/misc/bit_mem_stream.h)
* [**IStreamable**](#IStreamable)
* [**CIXml**](#CIXml)
* [**COXml**](#COXml)
* [CObjectSerializer](#CObjectSerializer)
	- [header](https://github.com/ryzom/ryzomcore/blob/ryzomclassic-develop/ryzom/common/src/game_share/object.h)
* 自定义对象的反序化序列化函数
	- 实现 ```void serial(NLMISC::IStream& stream);```
## IStream
## CMemStream
```c++
/**
 * Memory stream.
 *
 * How output mode works:
 * The buffer size is increased by factor 2. It means the stream can be smaller than the buffer size.
 * The size of the stream is the current position in the stream (given by lengthS() which is equal
 * to getPos()), because data is always written at the end (except when using poke()).
 * About seek() particularities: see comment of the seek() method.
 *
 * buffer() ----------------------------------- getPos() ---------------- size()
 *              data already serialized out        |
 *                                              length() = lengthS()
 *
 * How input mode works:
 * The stream is exactly the buffer (the size is given by lengthR()). Data is read inside the stream,
 * at the current position (given by getPos()). If you try to read data while getPos() is equal to
 * lengthR(), you'll get an EStreamOverflow exception.
 *
 * buffer() ----------------------------------- getPos() ------------------------- size()
 *              data already serialized in                   data not read yet      |
 *                                                                               length() = lengthR()
 *
 * \seealso CBitMemStream
 * \seealso NLNET::CMessage
 * \author Olivier Cado, Vianney Lecroart
 * \author Nevrax France
 * \date 2000, 2002
 */
class CMemStream : public NLMISC::IStream
```
### CObjectVector
```c++
/**
 * The purpose of this class is to copy most (but not all) of stl vector<> features, without
 *	some of the speed/memory problems involved:
 *		- size of a vector<T> is 16 bytes typically. size of a CObjectVector is 8 bytes (only a ptr and a size).
 *		- CObjectVector<T>::size() is faster than vector::size()
 *		- CObjectVector<T>::resize() is faster because it do not copy from a default value, it just call the
 *			default constructor of the objects.
 *		- clear() actually free memory (no reserve scheme)
 *
 *	Object contructors, destructors, operator= are correctly called, unless
 *	EnableObjectBehavior template argument is set to false (default is true)
 *	In this case: ctor, dtor are not called, and operator=() use memcpy.
 *
 *	Of course some features are not implemented (for benefit of speed/memory):
 *		- no reserve() scheme
 *
 *	Also, for now, not all vector<> features are implemented (iterator, erase etc...).
 *
 * \author Lionel Berenguier
 * \author Nevrax France
 * \date 2001
 */
template<class T, bool EnableObjectBehavior=true>	class CObjectVector
{
```
### CMemStreamBuffer
```c++
/** This class implement a copy on write behavior for memory stream buffer.
 *	The goal is to allow buffer sharing between CMemStream object, so that
 *	a CMemStream can be copied or passed by value as input/output parameter
 *	without copying the data buffer (thus making a CMemStrem copy almost free).
 *	This class reference a TMemStreamBuffer object with a smart pointer,
 *	when some code call the getBufferWrite() method to obtain write access
 *	to the memory buffer, if the ref count is more than 1, then we make a copy
 *	of the internal buffer for the current object.
 *
 *	\Author Boris Boucher
 */
class CMemStreamBuffer
{
public:
	typedef CObjectVector<uint8, false>	TBuffer;
```
## CBitMemStream
```c++
/**
 * Bit-oriented memory stream
 *
 * How output mode works:
 * In CMemStream, _BufPos points to the end of the stream, where to write new data.
 * In CBitMemStream, _BufPos points to the last byte of the stream, where to write new data.
 * Then _FreeBits tells the number of bits not used in this byte (i.e. (8-_FreeBits) is the
 * number of bits already filled inside this byte).
 * The minimum buffer size is 1: when nothing has been written yet, the position is at beginning
 * and _FreeBits is 8.
 *
 * How input mode works:
 * Same as in CMemStream, but some bits may be free in the last byte (the information about
 * how many is not stored in the CBitMemStream object, the reader needs to know the format of
 * the data it reads).
 *
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2001, 2003
 */
class CBitMemStream : public CMemStream
```
## IStreamable
```c++
/**
 * An Object Streamable interface. Any polymorphic class which want to use serial() in a polymorphic way, must derive
 * from this interface.
 * \author Lionel Berenguier
 * \author Vianney Lecroart
 * \author Nevrax France
 * \date 2000
 */
class IStreamable : public IClassable
{
public:
	virtual void		serial(IStream	&f) =0;
};
```
## CIXml
```c++
/**
 * Input xml stream
 *
 * This class is an xml formated input stream.
 *
 * This stream use an internal stream to input source xml code.
 * Use setStream to initialise it.
 \code
	// Check exceptions
	try
	{
		// File stream
		CIFile file;

		// open the file
		if (file.open ("input.xml"))
		{
			// XMl stream
			CIXml input;

			// Init, read all the input file...
			if (input.init (file))
			{
				// Serial the class
				myClass.serial (input);
			}

			// Close the file
			file.close ();
		}
		else
		{
			// File not found
		}
	}
	catch (const Exception &e)
	{
		// Something wrong appends
	}
 \endcode
 * \author Cyril 'Hulud' Corvazier
 * \author Nevrax France
 * \date 2001
 */
class CIXml : public IStream
```
## COXml
```c
/**
 * Output xml stream
 *
 * This class is an xml formated output stream.
 *
 * This stream use an internal stream to output final xml code.
 \code
	// Check exceptions
	try
	{
		// File stream
		COFile file;

		// Open the file
		file.open ("output.xml");

		// Create the XML stream
		COXml output;

		// Init
		if (output.init (&file, "1.0"))
		{
			// Serial the class
			myClass.serial (output);

			// Flush the stream, write all the output file
			output.flush ();
		}

		// Close the file
		file.close ();
	}
 	catch (const Exception &e)
	{
	}
\endcode
 *
 * \author Cyril 'Hulud' Corvazier
 * \author Nevrax France
 * \date 2001
 */
class COXml : public IStream
```
## CObjectSerializer
```c

```
