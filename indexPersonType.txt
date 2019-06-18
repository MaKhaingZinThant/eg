@extends('layout')
@section('content')
<!DOCTYPE html>
<html>
<head>
    <title>Person Type List Form</title>
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/app.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/bootstrap.min.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/dataTables.bootstrap.min.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/jquery.dataTables.min.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/dataTables.bootstrap4.min.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/font-awesome-4.7.0/css/font-awesome.min.css'))}}">
    <link rel="stylesheet" type="text/css" href="{{(asset('css/fontawesome-5.8.2/fontawesome-free-5.8.2-web/css/fontawesome.min.css'))}}">
    <link rel="stylesheet" href="css/dataTables.checkboxes.css">
    <script type="text/javascript" src="{{(asset('js/app.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/jquery.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/jquery.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/jquery.dataTables.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/dataTables.bootstrap.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/dataTables.bootstrap4.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/bootstrap.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/jquery.datatables.min.js'))}}"></script>
    <script type="text/javascript" src="{{(asset('js/jquery.validate.js'))}}"></script>
    <script src="js/dataTables.checkboxes.min.js"></script>
</head>
<body>
    
<div class="container">
  <h1><center>Person Type List Data</center></h1><br>
    <div class="pull-right">
    <a class="btn btn-success" href="{{route('personTypeCN.create')}}" > Print </a>
    <a class="btn btn-success" href="javascript:void(0)" id="createNewProduct"> Create New Product</a>
    </div>
    <div class="input-group mb-3 responsive" style="width: 30%">
                            <select name="batch" id="batch" class="custom-select col-md-6 ">
                                <option value="1">Batch 1</option>
                                <option value="2">Batch 2</option>
                                <option value="3">Batch 3</option>
                                <option value="4">Batch 4</option>
                                <option value="5">Batch 5</option>
                            </select>
                            <button class="btn btn-light col-md-1 ml-1"><i class="fa fa-plus"></i></button>
    </div>
    <div class="table-responsive">
      <form id="frm-example" method="POST">
      <table class="table table-bordered  table-stripped table-hover data-table" width="99.5%">
        <thead style="background-color: #e6e6e6">
          <tr>
               <th></th>
                <th>Id</th>
                <th>Name</th>
                <th>Remark</th>
                <th width="280px">Action</th>
          </tr>
        </thead>
        <tbody>
        </tbody>
      </table>
      </form>
    </div>
  
<div class="modal fade" id="ajaxModel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h4 class="modal-title" id="modelHeading"></h4>
            </div>
            <div class="modal-body">
              <span id="form_result"></span>

                <form id="productForm" name="productForm" class="form-horizontal">
                   <input type="hidden" name="personId" id="personId">
                    <div class="form-group">
                        <label for="name" class="col-sm-2 control-label">Name</label>
                        <div class="col-sm-12">
                            <input type="text" class="form-control" id="name" name="personDesc" placeholder="Enter Name" value="" maxlength="50" required="">
                        </div>
                    </div>
                     <div class="form-group">

            <label for="active">Active:</label>

            <input type="checkbox" name="active"  id="active">
        </div>
     
                    <div class="form-group">
                        <label class="col-sm-2 control-label">Remark</label>
                        <div class="col-sm-12">
                            <textarea id="remark" name="remark" required="" placeholder="Enter Details" class="form-control"></textarea>
                        </div>
                    </div>
      
                    <div class="col-sm-offset-2 col-sm-10">
                     <button type="submit" class="btn btn-primary" id="saveBtn" value="create">Save changes
                     </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
<div id="confirmModal" class="modal fade" role="dialog">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">&times;</button>
                <h2 class="modal-title">Confirmation</h2>
            </div>
            <div class="modal-body">
                <h4 align="center" style="margin:0;">Are you sure you want to remove this data?</h4>
            </div>
            <div class="modal-footer">
             <button type="button" name="ok_button" id="ok_button" class="btn btn-danger">OK</button>
                <button type="button" class="btn btn-default" data-dismiss="modal">Cancel</button>
            </div>
        </div>
    </div>
</div>   
</body>
    
<script type="text/javascript">
  $(function () {
     
      $.ajaxSetup({
          headers: {
              'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
          }
    });
    
    var table = $('.data-table').DataTable({
        processing: true,
        serverSide: true,
        ajax: "{{ route('personTypeCN.index') }}",
        columns: [
            { className: "center",defaultContent: '<input type="checkbox">'},
            {data: 'personId', name: 'personId'},
            {data: 'personDesc', name: 'personDesc'},
            {data: 'remark', name: 'remark'},
            {data: 'action', name: 'action', orderable: false, searchable: false},
        ]
    });
     
    $('#createNewProduct').click(function () {
      $('#form_result').html('');

        $('#saveBtn').val("create-product");
        $('#personId').val('');
       
          $('#active').val('');
        $('#productForm').trigger("reset");
        $('#modelHeading').html("Create New Product");
        $('#ajaxModel').modal('show');
    });
    
    $('body').on('click', '.editProduct', function () {
      var product_id = $(this).data('id');
      $('#form_result').html('');

      $.get("{{ route('personTypeCN.index') }}" +'/' + product_id +'/edit', function (data) {
          $('#modelHeading').html("Edit Product");
          $('#saveBtn').val("edit-user");
          $('#ajaxModel').modal('show');
          $('#personId').val(data.personId);
          $('#name').val(data.personDesc);
          $('#active').val(data.active);
          $('#remark').val(data.remark);
      })
   });
    
    $('#saveBtn').click(function (e) {
        e.preventDefault();
        $(this).html('Sending..');
    
        $.ajax({
          data: $('#productForm').serialize(),
          url: "{{ route('personTypeCN.store') }}",
          type: "POST",
          dataType: 'json',
          success: function (data) {
              html = '<div class="alert alert-success">' + data.success + '</div>';
             $('#productForm').trigger("reset");
               $('#form_result').html(html);
                setTimeout(function(){
    $('#ajaxModel').modal('hide');
     $('.data-table').DataTable().ajax.reload();
    },1000);

                table.draw();

          },
          error: function (data) {
              html = '<div class="alert alert-success">' + data.success + '</div>';

              console.log('Error:', data);
              $('#saveBtn').html('Save Changes');
          }

      });
    });
    
   $('body').on('click', '.deleteProduct', function () {
     
        var product_id = $(this).data("id");
       $('#confirmModal').modal('show');
       $('#ok_button').text('Delete');
        $('#ok_button').click(function(){
        $.ajax({
            type: "DELETE",
            url: "{{ route('personTypeCN.store') }}"+'/'+product_id,
              beforeSend:function(){
                  $('#ok_button').text('Deleting...');
                 },

       success: function (data) {
      setTimeout(function(){
     $('#confirmModal').modal('hide');
     $('.data-table').DataTable().ajax.reload();
    }, 300);

                table.draw();
            },
            error: function (data) {
                console.log('Error:', data);
            }
        });
      });
    });
});




</script>
</html>
@endsection