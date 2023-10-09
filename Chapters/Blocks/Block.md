## Blocks: a Detailed Analysis
>>> [ 1 + 2 ]
>>> 3

[ :x | x + 2 ] value: 5 
>>> 7
>>> 3
[ 1 + 2 ] cull: 5 cull: 6
>>> 3
[ :x | 2 + x ] cull: 5 
>>> 7
[ :x | 2 + x ] cull: 5 cull: 3 
>>> 7
[ :x :y | 1 + x + y ] cull: 5 cull: 2
>>> 8
[ :x :y | 1 + x + y ] cull: 5 
>>> error because the block needs 2 arguments.
[ :x :y | 1 + x + y ] valueWithPossibleArgs: #(5)
>>> error because 'y' is nil and '+' does not accept nil as a parameter.
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'BlockExperiment'
	| t |
	t := 42.
	^ self executeBlock: [ t ]
	| t |
	t := 33.
	^ aBlock value	
>>> 42
	self assert: self setVariableAndDefineBlock equals: 42
	| t |
	t := 42.
	^ self executeBlock: [ t := 2008. t ]
	self assert: self setVariableAndChangingVariableBlock equals: 2008
	| t |
	^ String streamContents: [ :st |  
		t := 42.
		self executeBlock: [ st print: t. st cr. t := 99. st print: t. st cr ].
		self executeBlock: [ st print: t. st cr. t := 66. st print: t. st cr ].
		self executeBlock: [ st print: t. st cr. ]
	]
	self assert: self accessingSharedVariables equals: '42
	99
	99
	66
	66
	'
	instanceVariableNames: 'block'
	classVariableNames: ''
	package: 'BlockExperiment'
	
	^ String streamContents: [ :st |  
		| t |
		t := 42.
		block := [ st print: t ].
		t := 69.
		self executeBlock: block ]
	self assert: self variableLookupIsDoneAtExecution equals: '69'
	^ String streamContents: [ :st |  
		block := [ st << arg ].
		self executeBlockAndIgnoreArgument: 'zork']
	^ block value
	self assert: (self testArg5: 'foo') equals: 'foo'
	instanceVariableNames: 'block x'
	classVariableNames: ''
	package: 'BlockExperiment'
	x := 123.
	instanceVariableNames: 'x'
	classVariableNames: ''
	package: 'BlockExperiment'
	x := 69.
	^ aBlock value
	^ String streamContents: [ :st | 
			self executeBlockInAnotherInstance6: [ st print: self ; print: x ] ]
	^ NBexp2 new executeBlockInAnotherInstance6: aBlock
	self assert: self selfIsCapturedToo equals: 'NBexp>>#testSelfIsCapturedToo123'
	| tmp |
	tmp := 2. 
	[
		| tmp |
		tmp := 3 ]]
	| tmp |
	tmp := 2]
	| arg |
	^ arg
	arg := 42	
	tmp := 2
	 ]
	| collection |
	collection := OrderedCollection new.
	#(1 2 3) do: [ :index |
		| temp |
		temp := index.
		collection add: [ temp ] ].
	^ collection collect: [ :each | each value ]
	self assert: self blockLocalTemp asArray equals: #(1 2 3)
	| collection temp |
	collection := OrderedCollection new.
	#(1 2 3) do: [ :index | 
		temp := index.
		collection add: [ temp ] ].
	^ collection collect: [ :each | each value ]
	self assert: self blockOutsideTemp asArray equals: #(3 3 3)
	| a |
	[ a := 0 ] value.
	^ a
>>> 0	
	| a |
	a := 0.
	^ {[ a := 2] . [a]}
	| array |
	array := self twoBlockArray.
	self assert: array second value equals: 0.

	array first value.
	self assert: array second value equals: 2
	^ String streamContents: [:str | 
		str nextPutAll: 'one'; cr.
		0 isZero ifTrue: [ str nextPutAll: 'two'. ^ str contents ].
		str nextPutAll: 'not printed']
	self assert: self withExplicitReturn equals: 'one
	two'
	#(1 2 3 4) do: [:each |
					each = 3
						ifTrue: [^ 3]].
	^ 42
	self assert: self jumpingOut equals: 3
	x := 123.
	stream := '' writeStream 
	stream nextPutAll: aString; cr
	^ stream contents
	| res |
	self traceCr: 'start start'.
	res := self defineBlock.
	self traceCr: 'start end'.
	^ res
	| res |
	self traceCr: 'defineBlock start'.
	res := self arg: [ self traceCr: 'block start'.
                            1 isZero ifFalse: [ ^ 33 ].
                            self traceCr: 'block end'. ].
	self traceCr: 'defineBlock end'.
	^ res
	| res |
	self traceCr: 'arg start'.
	res := self executeBlock: aBlock.
	self traceCr: 'arg end'.
	^ res
	| res |
	self traceCr: 'executeBlock start'.
	res := self executeBlock: aBlock value.
	self traceCr: 'executeBlock loops so should never print that one'.
	^  res
   defineBlock start
      arg start
         executeBlock start
            block start
start end
	| res |
	self traceCr: 'defineBlock start'.
	res := self arg: [ thisContext home inspect.
					        self traceCr: 'block start'.
                            1 isZero ifFalse: [ ^ 33 ].
                            self traceCr: 'block end'. ].
	self traceCr: 'defineBlock end'.
	^ res
	  self value: [ ^ nil ]
	| val |
	[ :break |
	    1 to: 10 do: [ :i |
			         val := i.
			         val = 4 ifTrue: [ break value ] ] ] valuePassingEscapingBlock.
	Transcript show: 'Passed here!'.
	self assert: val equals: 4
	| val |
	[ :break |
	    1 to: 10 do: [ :i |
			         val := i.
			         val = 4 ifTrue: [ break value ] ] ] value: [^nil].
	self halt.
	self assert: val equals: 4
	^ [ ^ self ]

BExp new returnBlock value ~-> Exception
	| temp |
	temp := arg * 2.
	thisContext copy inspect.
	^ temp
homeContext := thisContext.
b1 := [| b2 |
          self assert: thisContext closure == b1.
          self assert: b1 outerContext == homeContext.
	      self assert: b1 home = homeContext.
          b2 := [self assert: thisContext closure == b2.
                  self assert: b2 outerContext closure outerContext == homeContext].
		     	  self assert: b2 home = homeContext.
          b2 value].
b1 value 
CompilationContext optionFullBlockClosure: true.
NBexp recompile: #blockLocalTemp.
(NBexp >> #blockLocalTemp) inspect
	| collection |
	collection := OrderedCollection new.
	#(1 2 3) do: [ :index |
		| temp |
		temp := index.
		collection add: [ temp ] ].
	^ collection collect: [ :each | each value ]
66 <7C> send: new
67 <D0> popIntoTemp: 0
68 <21> pushConstant: #(1 2 3)
69 <40> pushTemp: 0
70 <F9 02 01> fullClosure:compiledBlockNumCopied: 1
73 <7B> send: do:
74 <D8> pop
75 <40> pushTemp: 0
76 <F9 03 00> fullClosure:compiledBlockNumCopied: 0
79 <94> send: collect:
80 <5C> returnTop