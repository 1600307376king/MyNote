## js修改css样式
    
    <style>
        display:none;
    </style>
    
    <div id="test" class="test_c"> 你好 <div>
    
    <script>
        document.getElementById('test').style.display = 'inline';
    </script>