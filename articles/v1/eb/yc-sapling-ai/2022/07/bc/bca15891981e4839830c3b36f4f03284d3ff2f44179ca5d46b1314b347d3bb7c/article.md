---
schema_version: "1.0.0"
document_id: "bca15891981e4839830c3b36f4f03284d3ff2f44179ca5d46b1314b347d3bb7c"
company_key: "yc-sapling-ai"
company: "Sapling.ai"
source_id: "yc-sapling-ai-rss-3361843b7812"
canonical_url: "https://blog.sapling.ai/python-spelling-and-grammar-checkers/"
published_at: "2022-07-29T23:22:44+00:00"
first_seen_at: "2026-07-25T22:15:25.567776+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:45e902556041733ba2147c15173b7a48682e8d32cc29d171e7dd4755f177f83a"
---

# Python Spelling and Grammar Checkers

Using Python, you can check whether a sentence is correct in an automated manner, and provide suggestions for fixing up bad spelling and grammar. Processing text in this manner falls under the category of natural language processing (NLP). This article provides documentation on how to use Sapling as a Python spelling and grammar checker. Compare it to a list of a few other open-source Python libraries that might be more suitable for individual, non-commercial or open-source projects. For each library, check out the installation guide as well as some sample quick-start Python code that demonstrates how to use each SDK.


Contents


Toggle


##


Sapling


[Sapling](https://sapling.ai/docs) offers a deep neural network language model trained on millions of sentences. Outside of English, it also supports more than[10 different other languages in its language model and regular spell checking for more than 30 other languages](https://sapling.ai/multilingual) . This style of automated proofreading can identify fluency improvements as well as areas where a correct English word was used but would be considered incorrect in the context of the sentence.


For use cases that have security, privacy, or regulatory requirements, Sapling is HIPAA compliant, SOC2 compliant, and offers options for no-data retention or on-premise/self-hosted models. The on-premise version allows users to host the Sapling service in their own cloud or infrastructure so that processed data will stay in a specific geographical region or compute environment.


[Get an API key](https://sapling.ai/docs/api/api-access) for free and use it for testing or personal use. The free API key comes with limits on usage. The paid version of Sapling’s API has no throttling limits and costs money based on usage.


Sapling’s Python grammar checker is licensed by Apache 2.0: there are no restrictions on how you can use it. This license makes Sapling compatible with commercial software products that want to keep their code proprietary. An alternative JavaScript library also exists for backend applications that use a JavaScript runtime environment like Node.js, or for applications that have an HTML or web-based front end and process text from textareas and content editables. Sapling also has an HTTP API that can be called using other languages directly like PHP or Ruby (or any scripting language that supports HTTP POST and GET requests).


### Installing Sapling


- Visit[Sapling.ai](https://sapling.ai/user/register) to register an account.
- Visit the[dashboard](https://sapling.ai/user_settings) to generate an API key.
- Install Sapling’s SDK:


```text
python -m pip install sapling-py


```


If you don’t have pip, you can follow the instructions here to install it:[https://pip.pypa.io/en/stable/installation/](https://pip.pypa.io/en/stable/installation/)


### Sapling Usage


```text
from sapling import SaplingClient


api_key = '<API_KEY>'
client = SaplingClient(api_key=api_key)
edits = client.edits('Lets get started!', session_id='test_session')


''' returns ->
[{
"id": "aa5ee291-a073-5146-8ebc-c9c899d01278",
"sentence": "Lets get started!",
"sentence_start": 0,
"start": 0,
"end": 4,
"replacement": "Let's",
"error_type": "R:OTHER",
"general_error_type": "Other",
}]
'''
```


You can read more in:


- Python Docs:[https://sapling.readthedocs.io/en/latest/api.html](https://sapling.readthedocs.io/en/latest/api.html)
- Sapling Developer Docs:[https://sapling.ai/docs](https://sapling.ai/docs)


##


Open Source **Libraries and Licenses**


Before we discuss the next section of open source grammar checkers, take a quick overview of the licenses below. If you are familiar with open-source software licenses, you can skip this section.


For developers producing non-commercial products (like personal or research projects), open-source libraries may be a good choice. These are free and configurable. The trade-off between free and better performance or support may be an obvious one for those with budget constraints; however, it is also important to understand the restrictions.


Most open-source software licenses give users permission to modify and distribute the library in question. The open-source licenses for the Python grammar checkers on this list require modifications to the original code to be released publicly and under the same license.


- GNU Lesser General Public License (LGPL): Programs that incorporate LGPL code also need to be LGPL. You can get around this limitation by dynamically linking to LGPL code. If the LGPL code is ever distributed to an end user, the user needs to be able to re-link the application to their own version of the LGPL library. This can work on platforms that allow for library changes, like Windows, MacOS, Linux, but is not possible for others, like iOS. When building an internal tool, or a purely server-based SaaS tool, the distribution clause does not apply.
- Mozilla Public License (MPL): MPL is more permissive and allows for static linking of libraries. There are no re-linking requirements. This permissive license is easier to integrate into a commercial software product compared to GPL and LGPL.
- BSD, MIT, Apache: These licenses are permissive and grant use, distribution and relicensing rights, making them the easiest to use with commercial products.


##


LanguageTool


[LanguageTool](https://languagetool.org/proofreading-api) is an open-source (LGPL) rules-based grammar checker. It is available as a cloud HTTP end-point hosted by the LanguageTool company. This version has a free offering that has usage (20 requests per minute) and correction limits (30 misspelled words), as well as a paid offering with less restrictions. The cloud offering is currently neither SOC2 nor HIPAA compliant. You can also run the Java backend yourself and call it through Python bindings; however, having to maintain and run a separate Java server or process along with using the Python grammar checker client makes maintenance more complicated.


LanguageTool comes with a database of community-curated grammar rules for different languages. Keep in mind that some of the other languages may not have as good of grammar rule coverage as English does.


### Installing LanguageTool Backend


Local hosting of the backend is optional but can help keep text processing local for privacy and security reasons.


1. Download the Java executable:[https://languagetool.org/download/LanguageTool-stable.zip](https://languagetool.org/download/LanguageTool-stable.zip)
2. Install Java:[https://www.java.com/en/download/help/download_options.html](https://www.java.com/en/download/help/download_options.html)
3. Run the LanguageTool Backend:


```text
java -cp languagetool-server.jar org.languagetool.server.HTTPServer --port 8081 --allow-origin
```


### Installing LanguageTool Python Client


```text
pip install language-tool-python
```


### LanguageTool Usage


```text
import language_tool_python


tool = language_tool_python.LanguageTool('en-US')  # use a local server
tool = language_tool_python.LanguageToolPublicAPI('en-US') # or use public API


tool.correct('A sentence with a error in the Hitchhiker’s Guide tot he Galaxy')
# returns -> 'A sentence with an error in the Hitchhiker’s Guide to the Galaxy'
```


If you are looking for an alternative open source python grammar checker that utilizes the LanguageTool API:


- language-check:[https://github.com/myint/language-check/](https://github.com/myint/language-check/)
- pyLanguagetool:[https://github.com/Findus23/pyLanguagetool](https://github.com/Findus23/pyLanguagetool)


##


Hunspell


[Hunspell](https://github.com/hunspell/hunspell) is a popular open-source spell checker that you have likely come across before because it is integrated by default into Firefox, Chrome, and LibreOffice. It has extended support for unicode and language peculiarities like compounding and complex morphology. The name of the library comes from the fact that it is based on MySpell and works with MySpell dictionaries. One of the first languages supported was Hungarian. This is a good spell checker to integrate if you value a library that is widely used and is actively maintained.


Hunspell is written in C++ but you can use it in Python as a spell checker through Cython bindings. Hunspell is licensed under 3 separate licenses: GPL/LGPL/MPL. The MPL license makes Hunspell more permissive and easier to integrate into commercial products compared to Aspell, another spell checker which we will describe later.


### Installing Hunspell


```text
sudo apt install autoconf automake autopoint libtool


git clone https://github.com/hunspell/hunspell.git
cd hunspell
autoreconf -vfi
./configure
make
sudo make install
sudo ldconfig
```


### Installing Hunspell Python Bindings


```text
pip install cyhunspell
```


### Hunspell Usage


```text
from hunspell import Hunspell
h = Hunspell()


h.spell('correct') # True
h.spell('incorect') # False
```


##


Aspell


[Aspell](http://aspell.net/) is an open-source spell checker that performs slightly better than Hunspell. In addition to spell checking, Aspell also has built-in functionality to suggest alternatives to words, even if they exist in the dictionary. These suggestions can be used to capture issues where a dictionary word is written, but may not be the intended word or is incorrect in context. Keep in mind though that Aspell does not do full grammar checking. While Aspell is a C++ library, you can use as a Python spelling checker through C++ bindings.


The wider adoption of Hunspell over Aspell is most likely due to Aspell being licensed under LGPL, which is less permissive than MPL. If you are building a non-commercial or backend Python application, Aspell is likely a better choice than Hunspell.


### Install Aspell


```text
git clone https://github.com/GNUAspell/aspell.git
cd aspell
./autogen
./configure --disable-static --enable-32-bit-hash-fun
make
make install


```


### Install an Aspell Dictionary


```text
# download dictionary from here https://ftp.gnu.org/gnu/aspell/dict/
cd aspell6-en-2019.10.06-0
./configure
make
make install


```


### Install Aspell Python Bindings


```text
pip install cyhunspell
```


### Aspell Usage


```text
import aspell


s = aspell.Speller(('lang', 'en_US'))
s.check('word') # correct word -> returns True
s.check('wrod') # incorrect -> returns False
s.suggest('wrod') # -> return suggestions for input
```


##


Building Your Own


Grammar checkers are more complex to build from the ground up, they require either maintaining a database of rules for matching against or enough data to train an effective machine-learning language model. Nowadays the most effective models are based off of neural nets, but statistical models can also be trained. Both the training and maintenance of your own grammar checker can be expensive. This path is preferable only if you want to invest in you or your team’s expertise in natural language processing.


### Building a Spelling Checker


Building a spell checker in Python that takes text and suggests spelling corrections for words can be done in fewer than 50 lines of code. Starting with a dictionary or a list of words, the algorithm looks up each word in the sentence. For words that are not in the dictionary, suggestions are generated based on edit distance (the number of characters that need to change) compared to dictionary words. If only a couple suggestions are shown, they are prioritized assuming that words that are lower in edit distance are more likely to be the intended word.


An example of this algorithm and relevant Python code has been posted by Peter Norvig, a prominent AI computer scientist, and author of the most popular AI textbook “ *Artificial Intelligence: A Modern Approach* “. You can read about his approach here:[“How to Write a Spelling Corrector](https://norvig.com/spell-correct.html) .


### Building a Statistical based Grammar Checker


Statistics-based grammar checkers share very similar architectures to Statistical Machine Translation. They break down words and phrases into statistical likelihoods and use that to predict whether sentences are correct or incorrect. If replacement words or phrases are deemed to be statistically more likely, corrections can be suggested.


Symspell is an MIT licensed spell correction and fuzzy search library. The original library is written in C#, but various Python ports of the library exist; some of them are linked in the original repository here[https://github.com/wolfgarbe/SymSpell](https://github.com/wolfgarbe/SymSpell) . This library can be used to train a statistical model on text and then used as a spell checker.


### Building a Neural Network based Grammar Checker


The neural network based grammar checker shares the same architecture as Neural Machine Translation. The steps required to build such a library from scratch are outside the scope of this blog post. However, some Python frameworks exist that can be used with pre-trained models. An example of this is the happy-transformer:[https://github.com/EricFillion/happy-transformer](https://github.com/EricFillion/happy-transformer) . Other frameworks like[PyTorch](https://pytorch.org/) and[TensorFlow](https://www.tensorflow.org/) can also be used to train your own language models.


##


The Best Python Spelling and Grammar Checker


Finding the optimal Python spelling and grammar checker will depend on your project requirements. Python’s support for HTTP POST and GET operations means that you can also use a non-Python HTTP API for this purpose. Popular grammar-checking services like Grammarly that do not have a Python or HTTP API were also not included. Likewise, we excluded spelling and grammar check APIs that do not provide Python support from this overview. You can visit[this page](https://blog.sapling.ai/javascript-spelling-and-grammar-checkers) for a comparison of JavaScript spelling and grammar checkers.


Library Pros Cons


Sapling – Serverless
– Multiple language support
– Neural network grammar checking – Costs money


Language Tool – Multiple language support – Costs money, or hosting resources


Hunspell – Serverless
– Multiple language support – No grammar checking


Aspell – Serverless
– Multiple language support
– More performant that Hunspell – LGPL license is more restrictive
– No grammar checking


Build your own – ML expertise as a competitive advantage – Engineering cost of training and maintenance
