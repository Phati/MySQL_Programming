#**MYSQL** Program to convert number in figures to words 

create database tempdb;
use tempdb;

-- -----Procedure for One Digit number-----
drop procedure if exists sp_one_digit;
delimiter $$
create Procedure sp_one_digit(num int,out result varchar(10))
begin
case num
when 1 then set result = 'one';
when 2 then set result = 'two';
when 3 then set result = 'three';
when 4 then set result = 'four';
when 5 then set result = 'five';
when 6 then set result = 'six';
when 7 then set result = 'seven';
when 8 then set result = 'eight';
when 9 then set result = 'nine';
else set result = 'zero';
end case; 
end ;
$$
delimiter ;

-- test case for sp_one_digit  
call sp_one_digit(1,@res);
select @res;

-- ---Procedure for Two Digit number---
drop procedure if exists sp_two_digit;
delimiter $$
create Procedure sp_two_digit(in num int,out result varchar(20))
begin 
declare n int;
declare next_digits varchar(20);

-- if number is really a two digit
if (num > 9 and num < 100) then

-- to account for numbers between 10 and 20
if num < 20 then
case num
when 10 then set result = 'ten';
when 11 then set result = 'eleven';
when 12 then set result = 'twelve';
when 13 then set result = 'thirteen';
when 14 then set result = 'fourteen';
when 15 then set result = 'fifteen';
when 16 then set result = 'sixteen';
when 17 then set result = 'seventeen';
when 18 then set result = 'eighteen';
when 19 then set result = 'nineteen';
end case;
end if;
-- end if for numbers < 20

-- to account for numbers from 20 to 99
if num >= 20 then 
set n = floor(num/10); 
case n
when 2 then set result = 'twenty';
when 3 then set result = 'thirty';
when 4 then set result = 'forty';
when 5 then set result = 'fifty';
when 6 then set result = 'sixty';
when 7 then set result = 'seventy';
when 8 then set result = 'eighty';
when 9 then set result = 'ninety';
end case;
call sp_one_digit(num%10,next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result  = concat(result,' ',next_digits);
end if;
-- end if for numbers from 20 to 99

-- if the number is less than two digits
else
call sp_one_digit(num,result);

end if; 
end$$
delimiter ;

-- test case for sp_two_digit  
call sp_two_digit(22, @res);
select @res; 

-- Procedure for Three digit Number--
drop procedure if exists sp_three_digit;
delimiter $$
create Procedure sp_three_digit(in num int,out result varchar(50))
begin 
declare n int;
declare next_digits varchar(20);

-- if number is really a three digit number
if num > 99 and num <1000 then 
set n = floor(num/100);
call sp_one_digit(n,result);
call sp_two_digit((num%100),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'hundred ', next_digits);

-- if number is less than a three digit number
else
call sp_two_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_three_digit  
call sp_three_digit(222, @res);
select @res;

-- ---Procedure for Four digit Number---
drop procedure if exists sp_four_digit;
delimiter $$
create Procedure sp_four_digit(in num int,out result varchar(50))
begin 
declare n int;
declare next_digits varchar(50);
-- if number is really a four digit number
if num > 999 and num < 10000 then 

set n = floor(num/1000);
call sp_one_digit(n,result);
call sp_three_digit((num%1000),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'thousand ', next_digits);

-- if number is less than a four digit number
else
call sp_three_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_four_digit  
call sp_four_digit(2222, @res);
select @res;

-- -----Procedure for Five digit Number----
drop procedure if exists sp_five_digit;
delimiter $$
create Procedure sp_five_digit(in num int,out result varchar(50))
begin 
declare n int;
declare next_digits varchar(50);
-- if number is really a five digit number
if num > 9999 and num < 100000 then 

set n = floor(num/1000);
call sp_two_digit(n,result);
call sp_three_digit((num%1000),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'thousand ', next_digits);


-- if number is less than a five digit number
else
call sp_four_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_five_digit  
call sp_five_digit(12345, @res);
select @res;

-- ---Procedure for six digit number----
drop procedure if exists sp_six_digit;
delimiter $$
create Procedure sp_six_digit(in num int,out result varchar(100))
begin 
declare n int;
declare next_digits varchar(50);
-- if number is really a six digit number
if num > 99999 and num < 1000000 then 
set n = floor(num/100000);
call sp_one_digit(n,result);
call sp_five_digit((num%100000),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'Lakh ', next_digits);


-- if number is less than a six digit number
else
call sp_five_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_six_digit  
call sp_six_digit(123456, @res);
select @res;

-- --Procedure for seven digit Number--
drop procedure if exists sp_seven_digit;
delimiter $$
create Procedure sp_seven_digit(in num int,out result varchar(100))
begin 
declare n int;
declare next_digits varchar(100);
-- if number is really a seven digit number
if num > 999999 and num < 10000000 then 
set n = floor(num/100000);
call sp_two_digit(n,result);
call sp_five_digit((num%100000),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'Lakh ', next_digits);

-- if number is less than a seven digit number
else
call sp_six_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_seven_digit  
call sp_seven_digit(1234567, @res);
select @res;

-- ----Procedure for eight digit Number-----------
drop procedure if exists sp_eight_digit;
delimiter $$
create Procedure sp_eight_digit(in num int,out result varchar(150))
begin 
declare n int;
declare next_digits varchar(120);
-- if number is really a eight digit number
if num > 9999999 and num < 100000000 then 
set n = floor(num/10000000);
call sp_one_digit(n,result);
call sp_seven_digit((num%10000000),next_digits);
if next_digits = 'zero' then set next_digits=''; end if;
set result = concat(result,' ', 'Crore ', next_digits);

-- if number is less than a eight digit number
else
call sp_seven_digit(num,result);
end if;
end$$
delimiter ;

-- test case for sp_eight_digit  
call sp_eight_digit(12345679, @res);
select @res;

-- ---Procedure for converting Figure to words------

drop procedure if exists sp_fig_to_words;
delimiter $$
create procedure sp_fig_to_words(num int,out result varchar(150))
begin
call sp_eight_digit(num,result);
end$$
delimiter ;

call sp_fig_to_words(54673452,@res);
select @res;


> **Note:** You can easily expand the program to deal with higher values just by 
adding new stored procedures by making a few changes to the last stored procedure.











 
 