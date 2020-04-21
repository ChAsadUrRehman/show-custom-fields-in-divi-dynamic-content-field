## Show custom fields in divi dynamic content field option

![](https://raw.githubusercontent.com/ChAsadUrRehman/show-custom-fields-in-divi-dynamic-content-field/master/divi%20custom%20fields%20in%20dynamic%20fields%20options.PNG)

By default Divi allows only few selected fields and custom fields created using ACF to show in dynamic content options. you can make any custom field working with Divi Dynamic cntent option by just adding few lines in dynamic-content.php file

Steps:
1. open YOURWEB/wp-content/themes/Divi/includes/builder/feature
2. edit **dynamic-content.php** and change line 553 from 
  ```php
  'group'    => __( 'Custom Fields', 'et_builder' ),
  ```
  to
 ```php
 'group'    => __( get_post_type($post_id), 'et_builder' ),
 ```
 3. Chnage code from line 605,606 and 607 (if block) from 
 ```php
 if ( 'display' === $context || et_pb_is_allowed( 'read_dynamic_content_custom_fields' ) ) {
		$custom_fields = et_builder_get_custom_dynamic_content_fields( $post_id );
	}
  ```
to 
```php
 if ( 'display' === $context || et_pb_is_allowed( 'read_dynamic_content_custom_fields' ) ) {
		$args = array(
		'public'   => true
		);
		$all_post_types = get_post_types( $args);
		$all_posts_ids = get_posts( array(
			'fields'         => 'ids', // only return post ID´s
			'posts_per_page' => '-1',
			'post_type'      => $all_post_types,
		));
		foreach($all_posts_ids as $post_id){
			$custom_fields_new = et_builder_get_custom_dynamic_content_fields( $post_id );
			$custom_fields = array_merge( $custom_fields, $custom_fields_new );
		}
	}
  ```
  
  4. Save file and enjoy :-) 
