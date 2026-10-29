docker build -t greeter:v1 lessons/02-docker-basics/homework/greeter/
docker run -d --name greeter -p 9000:8000 greeter:v1
-e GREETING=Shalom
JSON: {massage:"shalom,Rivka!"}
כל השלבים עד שלב העתקת הapp.py יסומנו כcached ומשם והלאה ייבנו מחדש.
