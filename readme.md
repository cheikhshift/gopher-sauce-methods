# Tutorial on template pipelines
Add functionality to your webapp pages.

![enter image description here](http://www.unixstickers.com/image/data/stickers/golang/Golang%20mashup.sh.png)

### Benefits 

 - Give your web pages full access to your Go lang packages.

### Requirements

- Go lang
- [Gopher Sauce](https://github.com/cheikhshift/Gopher-Sauce/blob/master/readme.md)

## Getting started
(unix commands)
					
	mkdir new-project
	cd new-project		
	gos --make

Please make sure your current working directory is `new-project` (or any other name).

## Create /index page
Create new template file :
		
	touch web/index.tmpl

Contents of file (bootstrap 4 starter) : 

		 <!DOCTYPE html>
			<html lang="en">
			  <head>
			    <!-- Required meta tags -->
			    <meta charset="utf-8">
			    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
			
			    <!-- Bootstrap CSS -->
			    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css" integrity="sha384-rwoIResjU2yc3z8GV/NPeZWAv56rSmLldC3R/AZzGRnGxQQKnKkoFVhFQhNUwEyJ" crossorigin="anonymous">
			  </head>
			  <body>
			   
			
			    <!-- jQuery first, then Tether, then Bootstrap JS. -->
			    <script src="https://code.jquery.com/jquery-3.1.1.slim.min.js" integrity="sha384-A7FZj7v+d/sdmMqp/nOQwliLvUsJfDHW+k9Omg/a/EheAdgtzNs3hpfag6Ed950n" crossorigin="anonymous"></script>
			    <script src="https://cdnjs.cloudflare.com/ajax/libs/tether/1.4.0/js/tether.min.js" integrity="sha384-DztdAPBWPRXSA/3eYEEUWrWCy7G5KFbe8fFjk5JAIxUYHKkDx6Qin1DkWx51bBrb" crossorigin="anonymous"></script>
			    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/js/bootstrap.min.js" integrity="sha384-vBWWzlZJ8ea9aCX4pEW3rVHjgjt7zpkNpZk+02D9phzyeVkE+jo0ieGizqPLForn" crossorigin="anonymous"></script>
			  </body>
			</html>

## Add SendEmail method
Edit `gos.gxml` :

	nano gos.gxml


Add this tag within your `<methods>` tag. This will create a new pipeline for your templates to use.

	<method name="SendEmail" var="to,from" return="string">
		fmt.Println("Writing to backend ->" + from.(string))
		return to.(string)
	 </method>

** Make sure to cast your var(s) before using them. `to` => `to.(string)`. Replace `fmt.Println` with your favorite Golang Email library.

### Add form to web/index.tmpl

Within your `web/index.tmpl` add this form tag. 

	<form class="list-group list-group-item" action="" method="POST">
        <p><input type="text" name="Email" required  /></p>
        <p><input type="submit" class="btn btn-primary btn-lg" value="Send" /></p>
    </form>

### Add pipeline to web/index.tmpl

Prior to your `<form>` add these snippets :

Get email :

	 {{ $email := .R | form "Email" }}

Verify if input is not empty and send email :

	{{ if neq $email ""}}
	    {{SendEmail $email "mailer@localhost"}}
	    Sending email to {{ $email }}.
	{{end}}


 
### Testing
Run project :

	gos --run

Visit page : [localhost:8080](http://localhost:8080/)

Download this Repository and run `gos --run` once in directory.