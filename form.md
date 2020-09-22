## Custom form that store input in database WordPress

### MySQL Code

```sh

CREATE TABLE `wpformstore` (
  `ID` int(11) NOT NULL AUTO_INCREMENT,
  `Name` varchar(255) NOT NULL,
  `Email` varchar(255) DEFAULT NULL,
  `Service` varchar(25) DEFAULT NULL,
  `Date` date DEFAULT NULL,
  `Address` varchar(255) DEFAULT NULL,
  `Phone` int(11) DEFAULT NULL,
  `Message` varchar(100) DEFAULT NULL,
  `status` varchar(10) NOT NULL DEFAULT 'start',
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB  DEFAULT CHARSET=latin1 AUTO_INCREMENT=12 ;

```

```sh
/*
Template Name: Wordpress Form Page
*/

```

```sh

<?php
/**
/* Template Name: Wordpress Form Page
*
*
*	@package WordPress
*/
get_header(); ?>
<?php
        if (!empty($_POST)) {
        global $wpdb;
            $table = wpformstore;
            $data = array(
                'Name'    => $_POST['your-name'],
				'Email'    => $_POST['your-email'],
				'Service'    => $_POST['Service'],
				'Date'    => $_POST['date'],
				'Address'    => $_POST['address'],
				'Phone'    => $_POST['tel'],
				'Message'    => $_POST['your-message']
            );
            $format = array(
                '%s','%s','%s','%s','%s','%s','%s'
            );
            $success=$wpdb->insert( $table, $data, $format );
            if($success){
				echo '<h3>Your request successfully Submitted</h3>' ; 
			}
		}
		else   { ?>
	<div id="primary" class="content-area container">
		<form method="post">
		<p><label> Your Name<br />
			<input type="text" name="your-name" value="" size="40" /></label></p>
		<p><label> Your Email<br />
			<input type="email" name="your-email" value="" size="40"/></label></p>
		<p><label> Select Service<br />
			<select name="Service">
				<option value="Website">Website</option>
				<option value="Digital Marketing">Digital Marketing</option>
				<option value="SEO">SEO</option>
				</select>
			</label></p>
		<p><label> Date <br />
			<input type="date" name="date" value="" placeholder="Select Date" /></label></p>
		<p><label> Your Address<br />
			<textarea name="address" cols="40" rows="10"></textarea></label></p>
		<p><label> Your Phone Number<br />
			<input type="tel" name="tel" value="" size="40" placeholder="Mobile Number" /></label></p>
		<p><label> Your Message <br />
			<textarea name="your-message" cols="40" rows="10" ></textarea></label></p>
		<p><input type="submit" value="Send" class="wpcf7-form-control wpcf7-submit" /></p>
		</form>

		</main><!-- .site-main -->
	</div><!-- .content-area -->
	<?php }  ?>
<?php get_footer(); ?>


```
