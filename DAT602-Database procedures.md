# DAT602-Database procedures

With this milestone, I updated the CRUB table to change the action name which would fit the procedure in the database. Furthermore, when getting in SQL code the complete a procedure, it is better to keep it simple and follow the ACID rule which I just learn through this milestone. The procedure should also have an error handler method or guard for easier debugging.

Based on my previous experience with another programming language, my code may not be following and utilising all the functions which are provided by SQL but it solves some of the challenges I tackle during the process of building the database for the game application For example :

```mysql
if exists( select* from Users
where Users.email = Email) then
	select 
    Users.password,
    Users.admin_check ,
	Users.login_check ,
   Users.login_attempt
    into  @password, @admin_check, @login_check , @login_attempt
    from Users
    where Users.email = inputEmail;
    if @login_check = 1 then
		select "User already login" as message;
	elseif @login_attempt = 0 then
		select "Three failed login attempt, account is locked" as message;
    elseif @password = inputPassword then
		update Users
		set Users.login_check = 1,
			Users.login_attempt = 3
        where Users.email = inputEmail;
		if @admin_check = true then
			select 'Hello admin' as message;
			else
			select 'hello user' as message;
        end if;
	else
    update Users
		set Users.login_attempt = @login_attempt - 1
        where Users.email = inputEmail;
		select 'wrong password' as message;
    end if;
    else 
		select 'There is no user with this email address' as message;
    end if;
```

The code used mainly coding blocks of if-else statements which is common in most programming languages but in my opinion, if I used table comparisons and data manipulation techniques such as `UNION`, and `JOIN` then the code will be cleaner and more suitable with SQL database.

Moreover, due to my lack of experience while working with SQL but thankfully my tutor pointed out that my design is a bit complex and contains some unnecessary methods such as creating a map for each session which can put a burden on the system performance with the increase of user number. Furthermore, the map created is a new table which will require further indexing to get data from different tables. Although a bit disappointed as I think it is really amazing to write dynamic SQL code which will be able to produce different results based on the input as well as trigger actions such as creating and deleting different tables.

Below is an example of dynamic SQL which is convenient but could be prone to errors, especially with an SQL injection attack. The procedure would drop the table which contains map information for the current session by taking the session ID information.

```mysql
SET SQL_SAFE_UPDATES = 0;
	select session_map into @MapName from Sessions where Sessions.sessionID = sessionID;
    set @MapQuery = concat('drop table if exists ',@MapName);
    PREPARE stmt FROM @MapQuery;
    EXECUTE stmt;
	delete from Sessions where Sessions.sessionID= sessionID; 
SET SQL_SAFE_UPDATES = 1;
	select 'Remove session Complete' as message;
```

Overall, while my SQL script function and produce the required result, I would like to archive further understanding and application of SQL syntax before the end of this course.