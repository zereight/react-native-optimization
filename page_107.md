import '@formatjs/intl-getcanonicallocales/polyfill';
import '@formatjs/intl-locale/polyfill';
import '@formatjs/intl-numberformat/polyfill';
import '@formatjs/intl-numberformat/locale-data/en';
import '@formatjs/intl-datetimeformat/polyfill';
import '@formatjs/intl-datetimeformat/locale-data/en';
import '@formatjs/intl-pluralrules/polyfill';
import '@formatjs/intl-pluralrules/locale-data/en';
import '@formatjs/intl-relativetimeformat/polyfill';
import '@formatjs/intl-relativetimeformat/locale-data/en';
import '@formatjs/intl-displaynames/polyfill';
Importing all possible Intl polyfills is not always necessary
The support for this API has been limited in Hermes, but fortunately, last year, the Hermes 
team announced they were working on improving it. A year later, some of those polyfills can 
be removed! At the time of writing this guide (January 2025), here are the available APIs:
Intl APIHermes support
Intl.Collator 
✅
Intl.DateTimeFormat
✅
Intl.NumberFormat
✅
Intl.getCanonicalLocales()
✅
Intl.supportedValuesOf()
✅
Intl.ListFormat
❌
Intl.DisplayNames
❌
Intl.Locale
❌
Intl.RelativeTimeFormat
❌
Intl.Segmenter
❌
Intl.PluralRules
❌
With that new knowledge, we can reduce the number of polyfills we import into our project:
-import '@formatjs/intl-getcanonicallocales/polyfill';
 import '@formatjs/intl-locale/polyfill';
-import '@formatjs/intl-numberformat/polyfill';
-import '@formatjs/intl-numberformat/locale-data/en';
-import '@formatjs/intl-datetimeformat/polyfill';
-import '@formatjs/intl-datetimeformat/locale-data/en';
 import '@formatjs/intl-pluralrules/polyfill';
 import '@formatjs/intl-pluralrules/locale-data/en';
 import '@formatjs/intl-relativetimeformat/polyfill';
 import '@formatjs/intl-relativetimeformat/locale-data/en';
 import '@formatjs/intl-displaynames/polyfill';
Importing all possible Intl polyfills is not always necessary
Best Practice: Use Dedicated React Native SDKs Over Web
The Ultimate Guide to React Native Optimization
118