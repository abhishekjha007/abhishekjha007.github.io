<?php
include("includes/db.php");
?>
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title> Product Insertion </title>
  <script src="https://cloud.tinymce.com/stable/tinymce.min.js"></script>
  <script>tinymce.init({ selector:'textarea' });</script>
</head>
<body bgcolor="lightgreen">
<form method="post"action="insert_product.php" enctype="multipart/form-data">
<table border="2px" cellspacing="0px" width="600px" height="630px" align="center" bgcolor="lightblue">

<tr align="center">
    <td colspan="2"><h1>Insert New Products :  </h1></td>
</tr>
<tr>
    <td align="right"><b>Product Title : </b></td>
    <td><input type="text" name="product_title" size="50" placeholder="Enter Product Title"/></td>
</tr>
<tr>
    <td align="right"><b>Product Category : </b></td>
    <td>
         <select name="product_cat">
             <option>Select a Category</option>
               <?php
         $get_cats = "select * from categories";
         $run_cats = mysqli_query($con, $get_cats);
    while($row_cats=mysqli_fetch_array($run_cats)) {
        
            $cats_id = $row_cats['cat_id'];
            $cats_title = $row_cats['cat_title'];

             echo "<option value='$cats_id'>$cats_title</option>";
            }
            ?> 
         </select>
    </td>
</tr>
<tr>
    <td align="right"><b>Product Brand : </b></td>
    <td>
         <select name="product_brand">
             <option>Select Brand</option>
             <?php
         $get_brands = "select * from brands";
         $run_brands = mysqli_query($con, $get_brands);
    while($row_brands=mysqli_fetch_array($run_brands)) {
        
            $brands_id = $row_brands['brand_id'];
            $brands_title = $row_brands['brand_title'];

             echo "<option value='$brands_id'>$brands_title</option>";
            }
            ?>               
         </select>
    </td>
</tr>
<tr>
    <td align="right"><b>Product Image 1 : </b></td>
    <td><input type="file" name="product_img1" /></td>
</tr>
<tr>
    <td align="right"><b>Product Image 2 : </b></td>
    <td><input type="file" name="product_img2" /></td>
</tr>
<tr>
    <td align="right"><b>Product Image 3 : </b></td>
    <td><input type="file" name="product_img3" /></td>
</tr>
<tr>
    <td align="right"><b>Product Price : </b></td>
    <td><input type="text" name="product_price" placeholder="Enter Price of the Product"/></td>
</tr>
<tr>
    <td align="right"><b>Product Description : </b></td>
    <td><textarea name="product_desc" cols="35" rows="10" placeholder="Write Product Description"></textarea></td>
</tr>
<tr>
    <td align="right"><b>Product Keywords : </b></td>
    <td><input type="text" name="product_keywords" size="50" placeholder="Write Product Keywords "/></td>
</tr>
<tr align="center">
    <td colspan="2"><input type="submit" name="insert_product" value="Insert Product"/></td>
</tr>
</table>
</form>
</body>
</html>


<?php

     if(isset($_POST['insert_product'])) {

// Text data variables.

     $product_title = $_POST['product_title'];
     $product_cat = $_POST['product_cat'];
     $product_brand = $_POST['product_brand'];
     $product_price = $_POST['product_price'];
     $product_desc = $_POST['product_desc'];
     $product_keywords = $_POST['product_keywords'];
     $status = 'on';       // Jo bhi data aayega wah screen pe display hoga.

//Image names.

     $product_img1 = $_FILES['product_img1']['name'];
     $product_img2 = $_FILES['product_img2']['name'];
     $product_img3 = $_FILES['product_img3']['name'];

//Creating Image Temporary names.

     $temp_name1 = $_FILES['product_img1']['tmp_name'];
     $temp_name2 = $_FILES['product_img2']['tmp_name'];
     $temp_name3 = $_FILES['product_img3']['tmp_name'];

if($product_title=='' OR $product_cat=='' OR $product_brand=='' OR $product_price=='' OR $product_desc=='' OR $product_keywords==''
OR $product_img1=='')   {

   echo "<script>alert('Please Fill all the Fields!!')</script>";
   exit();

}

else   {

//Uploading images to its folder
move_uploaded_file($temp_name1,"product_images/$product_img1");
move_uploaded_file($temp_name2,"product_images/$product_img2");
move_uploaded_file($temp_name3,"product_images/$product_img3");

$insert_product = "insert into products(cat_id,brand_id,date,product_title,
product_img1,product_img2,product_img3,product_price,product_desc,
status) values ('$product_cat','$product_brand',NOW(),'$product_title','$product_img1','$product_img2','$product_img3','$product_price','$product_desc','$status')";

$run_product = mysqli_query($con,$insert_product);

if($run_product)   {

  echo "<script>alert('Product Inserted Sucessfully ............!!!!!!!')</script>";

}

}


     }   
?>