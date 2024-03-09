# SVA
System Verilog Assertions

It has two Kind of Assertions:
1) Immediate Assertions
2) Concurrent Assertions
   1) Implication Operators
      1) Overlapped Implication
	  2) Non-overlapped Implication
   2) Repetition Operators
      1) Consecutive Repetition
      2) Go-To Repetition
	  3) Non-Consecutive Repetition
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
   5) Timing Window
      1) Indefinite Timing Window
   6) static Delay
      1) Variable Delay
   7) Throughout

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
   4) Implication Operators
      1) Overlapped Implication
	     1) The overlapped implication is denoted by the symbol "|->".
		 2) It will check the condition in same cycle of clock.
		 P1: property p;
		       @(posedge clk) a |-> b;
			 endproperty
	     A1: assert property(p);
		 P2: property delay;
		          @(posedge clk) a |-> ##2 b;
				endproperty
		 A2: assert property(delay);
		 
	  2) Non-overlapped Implication
	     1) The non-overlapped implication is denoted by the system "|=>".
		 2) It will check the condition in other cycle of clock.
		 P3: property pop;
		       @(posedge clk) a |=> b;
			 endproperty
	     A3: assert property(pop);
		 P4: property pup;
		       @(posedge clk) a |=> ##2 b;
			 endproperty
		 A4: assert property(pop);
	  3) Consecutive Repetition Operators
	     1) To check how many times pattern should comes of clocks specified.
		 2) Syntax "[*n]", below example shown.
		    property p;
			  @(posedge clk) a |-> ##2 b[*3];
			endproperty
			assert property(p);
	  4) Go To Repetition Operators
	     1) To check repetition of signal will match the number of times specified is not necessarily comes in continuous clock cycles.
		 2) Syntax "[->n]", below example shown.
		    property p;
			  @(posedge clk) a |-> ##2 b[->3];
			endproperty
			assert property(p);
	  5) Non-Consecutive Repetition Operators
	     1) To check repetition of signal or pattern but not in specific clock cycles.
		 2) Syntax "[=n]" below exampe shown.
		    property p;
			  @(posedge clk) a |-> ##2 b[=3];
			endproperty
			assert property(p);
	  6) $rose(boolean expression or signal name).
	     1) It will check on each and every positive edge of clock cycles.
		 2) mainly focus on signal change from LOW to HIGH.
		    sequence seq_rose;
			  $rose(hreq);
			endsequence
		    property p;
			  @(posedge clk) seq_rose;
			endproperty
			assert property(p);
	  7) $fell(boolean expression or signal name).
         1) It will check on each and every positive edge of clock cycles.
         2) mainly focus on signal change from HIGH to LOW.
           	sequence seq_fell;
			  $fell(hreq);
			endsequence
		    property p;
			  @(posedge clk) seq_fell;
			endproperty
			assert property(p);	 
	  8) $stable(boolean expression or signal name).
	     1) It will check on each and every positive edge of clock cycles.
		 2) mainly focus on signal did not chnage means should be stable for that clock cycles.
		    sequence seq_stable;
			  $stable(hreq);
			endsequence
		    property p;
			  @(posedge clk) seq_stable;
			endproperty
			assert property(p);
	  9) $past(signal name , number of clock cycles).
	     1) It  will check the signals from the previous clock cycle.
		    sequence seq_past;
			  a ##2 b;
			endsequence
		    property p;
			  @(posedge clk) seq_past |=> ($past(c,2) ==1);
			endproperty
			assert property(p);
	  10)$past(signal name, number of clock cycles, gating signal)
	     1) It will check the signals from the previous clock cycle, when gating signal has to be true before checking current expression.
		 sequence seq_past_gating;
			  a ##1 b;
			endsequence
		    property p;
			  @(posedge clk) seq_past_gating |=> ($past(c,2,d) ==1);
			endproperty
			assert property(p);
	  11)$onehot(expression).
	     property p;
		   @(posedge clk) $onehot(awid);
		 endproperty
		 assert property(p);
	  12)$onehot0(expression).
	     property p;
		   @(posedge clk) $onehot0(bid);
		 endproperty
		 assert property(p);
	  13)$isunknown(expression).
	     property p;
		   @(posedge clk) $isunknown(awvalid);
		 endproperty
		 assert property(p);
	  14)$countones(expression).
	     property p;
		   @(posedge clk) $countones(rstrb);
		 endproperty
		 assert property(p);
	  15)disable iff and ended consttruct
	     1) Suppose we want to check property with some condition should true we can use disable iff.
		 2) Suppose we want to concatenation the sequence, ending point should be synchronization point.
	     property p;
		   @(posedge clk) disable iff(rst) (awvalid |=> awready);
		 endproperty
		 assert property(p);
		 sequence seq_1;
		   a ##0 b;
		 endsequence
		 sequence seq_2;
		   b ##2 c;
		 endsequence
		 property p1;
		   @(posedge clk) seq_1.ended |-> ##3 seq_2.ended;
		 endproperty
		 assert property(p1);
	  16)Static Delay
	     1) We can give delay between signal or expression "##n".
	  17)Variable Delay
	     2) We can't give variable delay in sequence,property, assertion.
		 property p;
		   int_delay = 4;
		   a ##int_delay b;
		 endproperty
		 assert property(p);
		 3) this above example will give compilation error.
	  18)Throughout 
	     1) It will check a condition that must hold true throughout the duration of the assertion evaluation.
	     assert property ( @(posedge clk) disable iff (!reset) 
                           (my_signal[*2:5] throughout {3{3}}));
		

	  
	  
