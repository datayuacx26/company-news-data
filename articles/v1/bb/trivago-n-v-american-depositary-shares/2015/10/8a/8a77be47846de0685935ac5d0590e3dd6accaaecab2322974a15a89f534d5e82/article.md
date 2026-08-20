---
schema_version: "1.0.0"
document_id: "8a77be47846de0685935ac5d0590e3dd6accaaecab2322974a15a89f534d5e82"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-10-06-python_receipt_parser/"
published_at: "2015-10-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:37.879348+00:00"
content_hash: "sha256:502077d17cc6bd81d9a1d50a2381e5d517a17b668de62cd73e30ad053febce2a"
---

# Writing a Fuzzy Receipt Parser in Python

Last weekend, the Python Hackathon Düsseldorf took place at trivago’s office. Although we were only five people we had a lot of fun. I took the chance to brush up my Python skills a little bit. Also I wanted to scratch an itch that was bugging me for a long time: our housekeeping book.


You see, my girlfriend and I try to keep an overview of all our expenses. We learned that the hard way when we both were students and tight on money. In our opinion it is useful to know how much money we spend on things like food and fuel per month.


The problem is that I’m sloppy when it comes to keep the housekeeping book up to date. I tend to postpone the task. As a consequence we have a huge pile of receipts waiting to be transcribed in a box in our apartment.


There’s always a reason for procrastination. In this case, part of me thinks that a human should not need to do this boring task. I tend to make transcription mistakes and it is a waste of time to calculate the sum of expenses at the end of each month. Time that I could spend on better things; like teaching a computer how to do it.
A hackathon project was born.


## The bucket list


My little tool needs to fulfill these requirements:


- Scan receipts from shops like supermarkets and gas stations
- Detect the shop name, the date and the total. The precision rate should be above 90%. Otherwise I would spend more time to fix the tool than is needed to transcribe the data by hand.
- Write the output to a CSV file. This way we can create a graph out of it using GnuPlot or any spreadsheet program. We could also be fancy and use Elasticsearch and Kibana.


## Getting to work


First of all, I needed to scan a receipt. Any flatbed scanner would suffice here, but I wanted to make scanning as painless as possible.


I could have used a phone camera but it’s tricky to position it exactly above the receipt. Any tilting makes character recognition harder. The lighting needs to be correct as well. Also the result depends a lot on the phone’s resolution.


In the end I settled for an easier approach: At home I had an old Avision IS15 photo scanner lying around. It has a simple feed input so you don’t need to open a lid to use it. There was no way to get it to work on Mac or Linux, though. I tried SANE but the scanner was[not supported out of the box](http://www.sane-project.org/lists/sane-mfgs-cvs.html#Z-AVISION) . There was a driver for Windows but the first results looked not too promising. The scanner can also write data to a USB stick. For some reason the quality was much better in that case. Compare yourself:


After scanning, the images were rotated to the left. To position them upright again, I used a commandline tool called[ImageMagick](http://www.imagemagick.org/script/index.php) . Once installed, there’s a plethora of image operations to choose from. First I rotated the image:


```text
convert -rotate 90 input.jpg output.jpg
```


Next I read the contents of the receipt with Optical Character Recognition ( *OCR* ). For that I used[tesseract](https://github.com/tesseract-ocr/tesseract) , which is around for quite some time already. It was initially developed by HP in the 80s and 90s. Recently Google picked up development and improved it a lot. There’s also a nice Python wrapper called… well…[pytesseract](https://github.com/madmaze/pytesseract) .


Since using tesseract was so straightforward I just ran it as a shell command.
On my Mac it looks a bit like this:


```text
# Install tesseract with support for all languages
brew   install   tesseract   --all-languages


# Detect all characters in a German text.
# Tesseract will add a txt file ending to the output file
tesseract   -l   deu   input.jpg   output
```


Without any tweaking it already did an ok job. Here is some typical output:


Even though you might not be able to read the whole text, you can at least guess some parts. For example, the shop name is *real* , the sum is Euro 9,31 and the receipt date is, well… pretty hard to read. If you look very closely, it could be 4th of March 2015, but it’s tricky for a Computer to get right.


At that point I knew that I needed an iterative approach to achieve a high detection rate. So I wrote a Python script to import this receipt text from a file. Next I scanned 150 receipts, which took around 30 minutes to complete. After removing unusable scans, I had 127 unit tests to play around with.


For a start I tried to detect the shop name from the receipt. My first attempt was to search for some predefined shops. This worked pretty well. I could already detect 57 out of 127 names.


To improve the detection rate I only made three changes:


1. Add more shop names to my predefined list.
2. Fuzzy match the shop name if the exact string is not matched. I’ve achieved the best results with a matching accuracy of 60%.
3. If the shop name is completely unreadable, try to fuzzy match by the address.


For fuzzy matching I first tested with the Levenshtein distance. Soon after I found[get_close_matches()](https://docs.python.org/2/library/difflib.html#difflib.get_close_matches) in the difflib library. Here’s how I used it:


```text
def   fuzzy_find  (lines, keyword, accuracy  =  0.6  ):
"""
Returns the first line in lines that contains a keyword.
It runs a fuzzy match if 0 < accuracy < 1.0
"""
for   line   in   lines:
words: line.split()
# Get the single best match in line
matches: get_close_matches(keyword, words,   1  , accuracy)
if   matches:
return   line
```


With those simple steps I could detect the shop name with an accuracy of 98%. Nice!


Next up was the receipt date. This was trickier than I thought. Part of the problem was that tesseract made some character recognition mistakes. So I used[regex101](https://regex101.com/) to build a regular expression that covers most cases:


In the end I was able to achieve a success rate of 75% before I was running out of time. There was no time to improve my naive detection for the receipt sum.


## Bonus point: Increasing the detection rate


On the next day I took the train from Düsseldorf to Hamburg to visit the[Codetalks Conference 2015](https://www.codetalks.de/) . This gave me some time to play around with image preprocessing steps. ImageMagick comes with[a lot of built-in filters](http://www.imagemagick.org/script/command-line-options.php) . I adjusted the contrast and sharpened the text, which immediately improved the results:


```text
convert -auto-level -sharpen 0x4.0 -contrast
```


The following graph shows how the results improved over time. The runs before Number 27 are from the hackathon, the others are from the train ride.


It’s far from perfect but I’m quite happy with the result. With some more tweaking I’ll have a pretty decent fuzzy parser.


## Missing features


I have a spare Raspberry Pi lying around (as everybody has). It would be awesome to use it for receipt scanning.


It would also be nice to communicate with the scanner over a USB cable. For that I’d need to write a driver –[preferably in Python](https://github.com/walac/pyusb/blob/master/docs/tutorial.rst) . At the end of the hackathon I analyzed the wire connection between the scanner and the Windows driver, but I was running out of time. That said it would be fun to contribute the driver back to the SANE project.
To capture network traffic, you could use[Wireshark](https://wiki.wireshark.org/CaptureSetup/USB) or[SnoopyPro](http://sourceforge.net/projects/usbsnoop/) :


## Lessons learned


Turns out fuzzy parsing is hard. Especially handling all edge cases is a painstaking process. That said it’s fascinating how good Open Source tools have become. With some patience and a little tweaking you can already achieve solid results. Thanks to[Marc-André Lemburg](http://www.lemburg.com/files/mal/) for suggesting tesseract! The[code for my receipt parser is on Github](https://github.com/mre/receipt-parser) but don’t expect too much. It’s a hackathon project after all. If you want to improve it, I’m thankful for pull requests.


If you are interested in Python and live near Düsseldorf, Germany you can come to our monthly meetings. The next meeting will be on Wednesday, the 21st of October. Find more info[on the Usergroup website](http://pyddf.de/) .


Magic happens when the community comes together to try out new ideas.
Thanks to trivago for sponsoring Open Source projects!
