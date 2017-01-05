WAVE file [frogs-01-audacity.wav](./frogs-01-audacity.wav) was created with [Audacity](http://www.audacityteam.org/). I ran the file through JHOVE (Rel. 1.14.6) without specifying what format module to use:

    jhove frogs-01-audacity.wav
    
This gave me the following result:

    Jhove (Rel. 1.14.6, 2016-05-12)
     Date: 2017-01-05 12:57:16 CET
     RepresentationInformation: frogs-01-audacity.wav
      ReportingModule: BYTESTREAM, Rel. 1.3 (2007-04-10)
      LastModified: 2017-01-05 12:56:33 CET
      Size: 2051134
      Format: bytestream
      Status: Well-Formed and valid
      SignatureMatches:
       WAVE-hul
       WARC-kb
      MIMEtype: application/octet-stream

This result is not what I expected, because:

1. The *Format* designation is *bytestream* (expected *WAVE*).
2. Jhove reports a signature match for *WAVE*, but also one for *WARC*(!).

Compare this with running Jhove with the WAVE-hul module activated:

    jhove -m  WAVE-hul frogs-01-audacity.wav

Result:

    Jhove (Rel. 1.14.6, 2016-05-12)
     Date: 2017-01-05 13:05:07 CET
     RepresentationInformation: frogs-01-audacity.wav
      ReportingModule: WAVE-hul, Rel. 1.3 (2007-12-14)
      LastModified: 2017-01-05 12:56:33 CET
      Size: 2051134
      Format: WAVE
      Status: Not well-formed
      SignatureMatches:
       WAVE-hul
      InfoMessage: Chunk type 'ßþ+ü' ignored
       Offset: 131080
      InfoMessage: Chunk type '×þ´ü' ignored
       Offset: 131088
      InfoMessage: Chunk type 'ÿ§ü' ignored
       Offset: 131097
      InfoMessage: Chunk type 'Kü½ÿ' ignored
       Offset: 131106
      ErrorMessage: Invalid character in Chunk ID: Character = 0x07
       Offset: 131107
      MIMEtype: audio/x-wave
      Profile: PCMWAVEFORMAT
      WAVEMetadata: 
       AESAudioMetadata: 
        AnalogDigitalFlag: FILE_DIGITAL
        SchemaVersion: 1.02b
        Format: WAVE
        AudioDataEncoding: PCM audio in integer format
        ByteOrder: LITTLE_ENDIAN
        FirstSampleOffset: 44
        Use:
         UseType: OTHER
         OtherType: JHOVE_validation
        PrimaryIdentifier: frogs-01-audacity.wav
         IdentifierType: FILE_NAME
        Face: 
         TimeLine: 
          StartTime:
           FrameCount: 30
           TimeBase: 1000
           VideoField: FIELD_1
           CountingMode: NTSC_NON_DROP_FRAME
           Hours: 0
           Minutes: 0
           Seconds: 0
           Frames: 0
           Samples: 
            SampleRate: S44100
            NumberOfSamples: 0
           FilmFraming: NOT_APPLICABLE
            Type: ntscFilmFramingType
          Duration:
           FrameCount: 30
           TimeBase: 1000
           VideoField: FIELD_1
           CountingMode: NTSC_NON_DROP_FRAME
           Hours: 0
           Minutes: 0
           Seconds: 11
           Frames: 18
           Samples: 
            SampleRate: S44100
            NumberOfSamples: 1176
           FilmFraming: NOT_APPLICABLE
            Type: ntscFilmFramingType
          NumChannels: 2
          Stream:
           ChannelNum: 0
           ChannelAssignment: LEFT
          Stream:
           ChannelNum: 1
           ChannelAssignment: RIGHT
        FormatList:
         FormatRegion:
          BitDepth: 16
          SampleRate: 44100.0
       CompressionCode: PCM audio in integer format
       AverageBytesPerSecond: 176400
       BlockAlign: 4
       Data: 
        DataLength: 2050944
        
In this case Jhove correctly identifies the file as WAVE (and as a result it is validated accordingly, which reveals that one of the file's chunks contains an invalid character). Also, *SignatureMatches* does not contain *WARC* in this case.

So it seems that somethings goes wrong with the identification.