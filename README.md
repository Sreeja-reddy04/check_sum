# check_sum
## [RTL]
```bash
module checksum(data,c_sum,data_out);
input [15:0]data;
reg [23:0]data_append;
output reg [7:0]c_sum;
 reg [7:0]sum,sum1;
output reg data_out;
reg [8:0]temp_sum;
always@(*)
begin
temp_sum=data[15:8]+data[7:0];
sum=temp_sum[7:0]+temp_sum[8];
c_sum=~sum;
//c_sum[0]=~c_sum[0];// transmission error to get data_out=0;
data_append= {data[15:8],data[7:0],c_sum};
temp_sum = data[15:8] + data[7:0] + c_sum;
sum1 = temp_sum[7:0] + temp_sum[8];  
if(sum1==8'b11111111)
data_out=1'b1;
else
data_out=1'b0;
end
endmodule
```
## [Test bench]
```bash
module checksum_tb;

	// Inputs
	reg [15:0] data;
	// Outputs
	wire [7:0] c_sum;
	wire data_out;
	// Instantiate the Unit Under Test (UUT)
	checksum uut (
		.data(data), 
		.c_sum(c_sum),  
		.data_out(data_out)
	);
	initial begin
		data = 16'h0;
		#10;
        data=16'hFFFF;
		  #10;
		  data=16'd8;
		  #10;
		  data=16'd2;
		  #10;
		  data=16'd32;
		  #10;
		  data=16'hABCD;
		  #10;
		  data=16'hEEEE;
		  #10;
	     fork
		begin
		if (data_out!==1'b1)
		 $display("not working");
		else
		$display("working");
		end
      join
	end
      initial begin
      #200 $finish;
		end
		initial begin
   $monitor("data=%b,c_sum=%b,data_out=%b ",data,c_sum,data_out);
      end
endmodule
```
