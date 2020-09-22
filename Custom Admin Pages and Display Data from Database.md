## Creating Custom Admin Pages and Display Data from Database 

```sh
Added to Function.php
```


```sh


CREATE TABLE `wpdata` (
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

1, A menu entry – top-level or sub-level
2, The page content
3, Processing logic for forms – if needed.
```

```sh
add_action( 'admin_menu', 'my_admin_menu' );

function my_admin_menu() {
	add_menu_page( 'My Top Level Menu Example', 'Top Level Menu', 'manage_options', 
	'myplugin/myplugin-admin-page.php', 'myplguin_admin_page', 'dashicons-tickets', 6  );
}

```

```sh

add_action( 'admin_menu', 'my_admin_menu' );
	
	function customerview_admin_page(){
	?>
	<div class="wrap">
		<h2>Customer Details</h2>
		<?php
		global $wpdb;
		$customers = $wpdb->get_results("SELECT * FROM request ORDER BY ID DESC;");
		echo "<table class='widefat fixed'>";
		echo "<th>ID</th>";
		echo "<th>Name</th>";
		echo "<th>Email</th>";
		echo "<th>Service</th>";
		echo "<th>Date</th>";
		echo "<th>Address</th>";
		echo "<th>Phone</th>";
		echo "<th>Message</th>";
		echo "<th>Status</th>";
		foreach($customers as $customer){
			echo "<tr>";
			echo "<td><input type='text' name='ID' value=".$customer->ID." size='1' readonly></input></td>";
			$CusID = $customer->ID;
			echo "<td>".$customer->Name."</td>";
			echo "<td>".$customer->Email."</td>";
			echo "<td>".$customer->Service."</td>";
			echo "<td>".$customer->Date."</td>";
			echo "<td>".$customer->Address."</td>";
			echo "<td>".$customer->Phone."</td>";
			echo "<td>".$customer->Message."</td>";
			echo "<td>".$customer->status."</td>";
			echo "</tr>";
		}
		echo "</table>";
		?>
	</div>
	<?php
	}
	
	function my_admin_menu() {
		add_menu_page('Customer Request View', 'Customer Requests', 'manage_options',
		'myplugin/View_Customer_Details.php', 'customerview_admin_page', 'dashicons-tag', 6  );
	}
```


### Output
<img src="https://raw.githubusercontent.com/akashbadole/wordpress_help/master/Creating-Custom-Admin-Pages-and-Display.png" alt="Wordpress menu and data">
