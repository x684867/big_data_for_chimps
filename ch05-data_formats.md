# Chapter 5: Data Formats and Schemata

All of the following examples could be ambiguously referred to as "encoding":

* Compression: gz, LZO, Snappy, etc
* Serialization: the actual record container
* Stringifying: conversion to JSON, TSV, etc.
* Glyphing: binary stream (for example UTF8-encoded Unicode) to characters (TODO: make this correct)
* 
* Structured to model

[^fn] the worst example iis "node": a LAN node configured by a chef node running a Flume logical node handling graph nodes in a Node.js decorator.

there's only three data serialization formats you should use: TSV, JSON, or Avro. 

### TSV

For data exploration, wherever reasonable use tab-separated values (TSV). Emit one record per line, ASCII-only, with no quoting, and html enttity escaping where necessary. The advantage of TSV:

* you only have two control characters, NL and tab.
* It's restartable: a newline unambiguously signals the start of a record, so a corrupted record can't cause major damage.
* Ubiquitous: pig, Hadoop streaming, every database bulk import tool, and excel will all import it directly.
* Ad-hoc: Most importantly, it works with the standard unix toolkit of cut, wc, etc.
can use without decoding all fields

A stray quote mark can have the computer absorb the next MB of data I into one poor little field.
Don't do any quoting -- just escaping

Disadvantages

* Tabular data only with uniform schema
* Second extraction step to get text field contents

I will cite as a disadvantage, but want to stress how small it is: data size. Compression takes care of, 

Exercise: compare compression:

* Format: TSV, JSO, Avro, packed binary strings
* Compression: gz, bz2, LZO, snappy, native (block)
* Measure: map input, map output, reduce sort, reduce output, replicate, final storage size.

*best practice*

* Restartable - If you have a corrupt record, you only have to look for the next un-escaped newline 
* Keep regexes simple - Quotes and nested parens are hard,

Don't use CSV -- you're sure to have *some* data with free form text, and then find yourself doing quoting acrobatics or using a different format.

Use TSV when

* You're 
* 

Don't use it when

* Structured data
* Bulk documents
* 

## JSON

Individual JSON records stored one-per-line are your best bet for denormalized objects,

    {"text":"","user":{"screen_name":"infochimps",...},...}

for documents,

    {"title":"Gettysburg Address","body":"Four score and seven years ago...",...}

and for schema-free records:

    {"_ts":"","":"","data":{"_type":"ev.hadoop_job",...}}
    {"_ts":"","":"","data":{"_type":"ev.web_form",...}}

## structured to model.

TODO: receive should take an owner field 

Receiver:

    class Message
      def receive_user
      end
    end
    
    class User 
      def receive_
    Edn
    
    class Factory
      Def convert(obj)
        Correct type return it
        Look up factory
        Factory.receive obj
      End 
      
      def factory_for
        If factory then return it
        If symbol then get from identity map
        If string then Constantize it
        If hash then create model class, model_klass
        If array then union type
      end

Important that receeiver is not setter - floppy inassertive model with no sense of its own identity. Like the security line at the airport: horrible inexplicable things, more complicated then it seems like it should, but once past the security line you can g back to being a human again.


Do not make filedls that represent objects and keys -- if,there's a user fields put it in `user`, its id in user_id, and use virtual accessors to mask the difference.

This is a very rubyist way of thinking other languages pleass chime in.

aa

### tagged net strings and null-delimited documents 

Not restartable.

Can't represent a document with a null

## Crap Formats

### XML

 Once you've recapitulated the original raw data structure, 

It's unambiguously ser/deseriaalizimg 

Attributes, CDATA, model boundaries, document text

If you do it, consider emitting not with a serde but with a template engine. Pretty-print fields so can use cmdline tools

### N3 triples

Like most semweb things, is antagonistic to thought.

If you must deal with this, pretty-print the fields and ensure delimiters are clean


### Flat format

WALKTHROUGH: converting the weather fields. 

Flat formats are surprisingly innocuous; it's the contortions they force upon their tender that hurts.

Straightforward to build a regexp. Wukong gives you a flatpack stringifier.  Specify a format string as follows: 

    "%4d%3.2f\"%r{([^\"]+)}\""
    
It returns a MatchData object (same as a regexp does).

9999 as null (or other out-of-band): Override the receive_xxx method to knock those out, call super.

To handle the elevation fields, override the receive method: 


Note that we call super *first* here , because we want an int to divide; in the previous case, we want to catch 9999 before it goes in for conversion.
Wukong has some helpers for unit conversion too.

### Web log and Regexpable

WALKTHROUGH: apache web logs of course.
- 
Regexp to tuple.
Just capture substructure 

### Others

## Avro Schema

that there is no essential difference among

        File Format         Schema          API             RPC (Remote Procedure Call) Definition
        
        JPG                 CREATE TABLE    Twitter API     Cassandra RPC
        HTML DTD            db defn.
        
Avro says these are imperfect reflections for the same thing: a method to send data in space and time, to yourself or others. This is a very big idea [^1].

### Avro

## Glyphing (string encoding), Unicode,UTF-8

My best advice is 

* Never let *anything* into your system unless it is UTF8, UTF-16, or ASCII.
* Either:
  - Only transmit 7-bit ASCII characters in the range 0x20 (space) to 0x126 (~), along with 0x0a (newline) and 0x09 (tab) but only when used as record (newline) or field (tab) separators. URL encoding, JSON encoding, and HTML entity encoding are all reasonable. HTML entity encoding has the charm of leaving simple international text largely readable: "caf&eacute;" or "M&oumlaut;torh&eumlaut;ad" are more easily scannable than "caf\XX". Be warned that unless you exercise care all three can be ambiguous: &eacute;, (that in decimal) and (that in hex) are all the same.to make life grep'able, force your converter to emit exactly one string for any given glyph -- That is, it will not ship "0x32" for "a", and it will not ship "é" for "\XX"
  - Use unix-style newlines only.
  - Even With unique glyph coding, Unicode is still not unique: edge cases involving something something diacritic modifiers.
  - However complex you think Unicode is, it's slightly more hairy than that.
  -   URL encoding only makes sense when you're shipping urls anyway.
  - TODO: check those character strings for correctness. Also, that I'm using "glyph" correctly

## ICSS

ICSS uses 


### Schema.org Types
 

### Munging


    class RawWeatherStation
      field :wban_id
      # ...
      field :latitude
      field :longitude
    end
    
    class Science::Climatology::WeatherStation < Type::Geo::GovernmentBuilding
      field :wban_id
      field :
    end
    
    name:   weatherstation
    types:
      name:   raw_weather_station
      fields:
        - name:  latitude
          type:  float
        - name:  longitude
          type:  float
      # ...
      
### More      

ICSS gives


`_domain_id` and other identifiers




__________________________________________________________________________

[^1] To the people of the future: this might seem totally obvious. Trust that it is not. There are virtually no shared patterns or idioms across the systems listed here.

[^1] Every Avro schema file is a valid ICSS schema file, but Avro will not understand all the fields. In particular, Avro has no notion of 
and ICSS allows 