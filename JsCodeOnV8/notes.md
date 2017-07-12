**How V8 is executing JS**
- V8 only one optimization
- "Node Chakra Core" (MS's own node implementation?)
*parse*

node --trace-parse file.js (very verbose)
d8 --trace-parse file.js (only basic env)

(optimized) two phases, pre-parse and function parse, with heuristics e.g. when to really parse functions

optimize-js (github) for issues with parsing
ChromeTools..somewhere "V8 call [something] on timeline" or so

*interpret*
'Ignition'
find types, ...

*compile*
'Turbofan' (/'Crankshaft')
d8 --trace_opt file.js
generates machine code with types
-> changing types kills some optimization

*Object interpretation in V8*
Use of "hidden class"
adding properties to object changes generated hidden class
avoid by setting values of properties to at least null (maybe even in constructor)

*Object memory representation*
HeapSnapshot on ChromeDevTools
Use constructor for object literal (can be searched in DevTools)

Memory is allocated with extra space (e.g. for later added properties, extra space for numeric values)
optimize memory: instead of deleting properties, assign null or undefined

*Arrays*
extra space allocated as well
typed initially, on mismatch just add reference (but also convert all other values from simple to real object, e.g. 2.3 to Number(2.3))

*Final*
Don't optimize if unnecessary
