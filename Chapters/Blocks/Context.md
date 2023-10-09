## A First Look at Context
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