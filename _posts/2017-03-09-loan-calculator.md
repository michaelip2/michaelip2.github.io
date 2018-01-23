---
layout: post
title: "Loan Calculator"
date: 2017-03-09
---

This stored procedure shows the monthly payment and total payment by taking in three user input parameters: the loan amount, loan term in years, and the annual interest rate.
<pre><code>
CREATE PROCEDURE loanCalculator 
@amt float, -- principal loan amount
@yr smallint, -- loan term in number of years
@air float -- annual interest rate
AS
BEGIN
DECLARE @monthly_pmt float;

SET @monthly_pmt = (@amt * @air/1200)/( 1 - (1/(POWER((1 + @air/1200), @yr * 12))))
SELECT 
        concat('$', cast(@monthly_pmt as DECIMAL(10,2))) as 'Monthly Payment', 
        concat('$', cast(@monthly_pmt * @yr * 12 as DECIMAL(10,2))) as 'Total payment'
END


EXEC loanCalculator  @amt = 10000, @air = 6, @yr = 10;
</code></pre>

Let us say we were determining a monthly payment amount for a loan amount of $10,000 at an annual interest rate of 6% and for 10 years, the output shows (1 row):
<p><img src="https://michaelip2.github.io/images/2017-03-24 21_26_39-SQLQuery3.sql - DESKTOP-BB2A4CL_SQLEXPRESS.master (DESKTOP-BB2A4CL_Michael (56)).png"/></p>


