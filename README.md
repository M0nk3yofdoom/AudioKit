Objective-Csound
================

Potentially a very important part of the H4Y technology stack, Objective-Csound 
aims to eliminate the need for csd files and Csound-for-iOS hooks into code by 
representing every part of the Csound orchestra instruments as objects.  

TODO:

* Develop a way to write CSDExpression, ie. math in the Csound realm
* figure out how to get variables (think iDur(p3)) into paramArray
* Enhance CSDOrchestra to enable global outputs and instruments
* Set up Function Tables as subclasses a la CSDOpcodes
* Create more opcodes
* Develop examples
* Clean up the CSDManager's handling of new.csd, could be made quite a bit simpler
* Figure out how we really want to handle FINAL_OUTPUT, FINAL_OUTPUT_STEREO, etc.
* output of type CSDParam

BUGS:

* There is still some weirdness from stopping and starting different orchestras. 
Occassionally there is just no sound while other times it crashes.

NOTES:

* Although it's uncommon to have an fStatement as a p-Value, I believe it is 
legal, so should we be treating FunctionStatements as CSDParams also?

* Should there be subclasses of CSDParam as well?  That would allow us to have more 
informative signatures.

* Should think about standardizing the headers of the files.  I don't care much who 
wrote the file initially or when it was created.  To me that's all handled much 
better by the code repository, although I suppose if we create a new fresh repo at 
some point, then all is lost.

* CSDOscillator keeps a reference to NSString *output and sets it during init. convertToCSD looks for *output as an argument sent to the formatter.  CSDFoscili just doesn't have an ivar output and just uses "foscili" directly in the formatter of convertToCsd.  Do we want to keep a reference to output for comparison?  Maybe an enum at CSDConstants of all the available opcode types would be simpler for comparison and whatnot.

* Adding unit generators: is there an elegant way to generate a%, k%, and i% prefixes for output assignments (and their subsequent re-use in paramteres of other opcodes)? Ideally, an objective-csound user would set something like an (BOOL)audioRate flag and everything would be set for them when converting to csd, but this causes a problem when user wants to re-enter the output in the init argument of a newly created opcdode.

* making a,k,i assignments behind the scenes as part of scripting opcode id's and output generation is essential.  As I was making unitgen example, my output was a string set to "aSomething" and I was trying to plug the opcode into parameters that only took k.  The result was silence.  This should all be under the hood and automated after specifying the rate for an opcdoe and knowing what kind of affordances a parameter has by seeing its fastest subclass of CSDParam on autocomplete.  This also means that if something can take, a,k, OR i rate and the autocomplete says CSDParamA, then the it may also need to handle a CSDParamK object passed as argument. 

RECENT UPDATE NOTES:
* if wanting to set CSDParam to pValue corresponding to a note's duration, the pfield will always be 3.  Should be a final constant variable to hide the p-field number from the objective-c user.  However, by making a CSDConstant typedef of type PValueReservedTag, we reinforce how users should stylistically create typedefs with this naming convention for their variable p-fields kPValueTagSomething