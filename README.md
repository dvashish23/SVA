# SVA
System Verilog Assertions

It has two Kind of Assertions:
1) Immediate Assertions
2) Concurrent Assertions
   1) Implication Operators
      1) Overlapped Implication
	  2) Non-overlapped Implication
   2) Repetition Operators
      1) go-to repetition
	  2) non-consecutive repetition
   3) SVA Methods
      1) $rose
	  2) $fell
	  3) $stable
	  4) $past
	  5) $past with clock gating
	  6) $onehot
	  7) $onehot0
	  8) $isunknown
	  9) $countones
   4) Construct
      1) disable iff
	  2) ended
   5) static Delay
      1) Variable Delay
   6) Timing Window
      1) Indefinite Timing Window

1) Immediate Assertions
   1) Immediate Assertions checks for a condition at the current simulation time.
   2) An Immediate Assertions is the same as an "if..else" statement with assertion control.
   3) Immediate Assertions have to be placed in a procedural block 
   4) Syntax of Immediate Assertions
      label: assert(expression)
	  pass_statement;
	  else 
	  fail_statement;
   5) You can use multiple statement using "begin..end" block for e.g. 
	  label: assert(expression)
	  begin
	  statement_1;
	  statement_2;
	  end
	  else begin
	  statement_3;
	  statement_4;
	  end
   6) This statement you can use or its optional, because if you didn't write assert statement get fail then it automatically gives assert failure.
   7) Depending on your requirements, you can use any severity system task in either the fail or pass statement.
   8) Real Example   
      always@(posedge clk)
	  assert(a && b)

2) Concurrent Assertions
   1) Concurrent Assertions Checks for a condition at multiple clock cycles.
   2) We can write in procedural block, a module, an interface or a program.
   3) In this we can make sequence expression, property, assert.
      sequence user_name_of_sequence;
	  ...
	  ...
	  ...
	  endsequence
	  
	  property user_name_property;
	  ...
	  ...
	  ...
	  endproperty
	  
	  user_assert_name: assert property(user_name_property)
	                    pass_statement;
						else
						fail_statement;
	
