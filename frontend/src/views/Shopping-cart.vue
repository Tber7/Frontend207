<template>
    <!-- Phần tử gốc duy nhất -->
    <div>
      <!-- Phần thông tin sản phẩm -->
      <div class="container-sm w-50 rounded-top-4" style="height: 250px; background-color: white;">
        <div class="row">
          <div class="col-sm-6 row mt-5">
            <div class="col-6 text-center">
              <img src="/images/icon2.jpg" alt="" class="w-50">
            </div>
            <div class="col-6">
              <h6>Rubik Gan</h6>
              <p>Khuyến mãi</p>
              <select class="form-select w-75" aria-label="Default select example">
                <option selected>Trắng</option>
                <option value="1">Đen</option>
                <option value="2">Đỏ</option>
                <option value="3">Vàng</option>
              </select>
            </div>
          </div>
          <div class="col-sm-6 mt-5 text-end">
            <h5>1.290.000 <sup><u>đ</u></sup></h5>
            <p style="color: #757575;"><s>1.090.000 <sup><u>đ</u></sup></s></p>
            <br>
            <div class="input-group float-end w-25">
              <span class="input-group-btn">
                <button type="button" class="btn btn-secondary p-1">
                  <i class="fa-solid fa-minus"></i>
                </button>
              </span>
              <input type="text" class="form-control rounded-3" value="0">
              <span class="input-group-btn">
                <button type="button" class="btn btn-secondary p-1">
                  <i class="fa-solid fa-plus"></i>
                </button>
              </span>
            </div>
          </div>
        </div>
        <div class="row mt-4">
          <div class="col-sm-6 text-center">
            <p><b>Tạm tính</b>(2 sản phẩm):</p>
          </div>
          <div class="col-sm-6 text-end">
            <p><b>2.580.000 <sup><u>đ</u></sup></b></p>
          </div>
        </div>
      </div>
  
      <!-- Phần thông tin khách hàng -->
      <div class="container-sm w-50 mt-2" style="height: 200px; background-color: white;">
        <h6 class="pt-4 mb-3 ms-4">Thông tin khách hàng</h6>
        <div class="row ms-4">
          <div class="col-sm-2">
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
              <label class="form-check-label" for="flexRadioDefault1">Anh</label>
            </div>
          </div>
          <div class="col-sm-2">
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2" checked>
              <label class="form-check-label" for="flexRadioDefault2">Chị</label>
            </div>
          </div>
        </div>
        <div class="row g-3 mt-2 ms-4" style="width: 600px;">
          <div class="col">
            <input type="text" class="form-control rounded-4" style="height: 50px;" placeholder="Họ và tên">
          </div>
          <div class="col">
            <input type="text" class="form-control rounded-4" style="height: 50px;" placeholder="Số điện thoại">
          </div>
        </div>
      </div>
  
      <!-- Phần chọn hình thức nhận hàng -->
      <div class="container-sm w-50 mt-2" style="height: 500px; background-color: white;">
        <h6 class="pt-4 mb-3 ms-4">Chọn hình thức nhận hàng</h6>
        <div class="row ms-4">
          <div class="col-sm-3">
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault3">
              <label class="form-check-label" for="flexRadioDefault3">Giao tận nơi</label>
            </div>
          </div>
          <div class="col-sm-4">
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault4" checked>
              <label class="form-check-label" for="flexRadioDefault4">Nhận tại cửa hàng</label>
            </div>
          </div>
        </div>
        <div class="row g-3 mt-2 bg-light rounded-4 ms-4 p-3" style="width: 600px;">
          <div class="col-6">
            <select class="form-select rounded-4" aria-label="Default select example" style="height: 50px;">
              <option selected>Hồ Chí Minh</option>
              <option value="1">Hà Nội</option>
              <option value="2">Đà Nẵng</option>
              <option value="3">Cần Thơ</option>
            </select>
          </div>
          <div class="col-6">
            <select class="form-select rounded-4" aria-label="Default select example" style="height: 50px;">
              <option selected>Quận / Huyện</option>
              <option value="1">One</option>
              <option value="2">Two</option>
              <option value="3">Three</option>
            </select>
          </div>
          <div class="col-6">
            <select class="form-select rounded-4" aria-label="Default select example" style="height: 50px;">
              <option selected>Phường / Xã</option>
              <option value="1">One</option>
              <option value="2">Two</option>
              <option value="3">Three</option>
            </select>
          </div>
          <div class="col-6">
            <input type="text" class="form-control rounded-4" placeholder="Số nhà, tên đường" style="height: 50px;">
          </div>
        </div>
        <div class="row g-3 rounded-4 ms-4 p-3" style="width: 600px;">
          <input type="text" class="form-control rounded-4" placeholder="Ghi chú" style="height: 50px;">
        </div>
        <div class="row g-3 rounded-4 ms-4 p-3" style="width: 600px;">
          <div class="form-check">
            <input class="form-check-input" type="checkbox" id="flexCheckDefault">
            <label class="form-check-label" for="flexCheckDefault">Gọi người khác nhận hàng</label>
          </div>
          <div class="form-check">
            <input class="form-check-input" type="checkbox" id="flexCheckChecked">
            <label class="form-check-label" for="flexCheckChecked">Chuyển danh bạ, dữ liệu qua máy mới</label>
          </div>
        </div>
      </div>
  
      <!-- Phần tổng kết và đặt hàng -->
      <div class="container-sm w-50 mt-2 rounded-bottom-4" style="height: 300px; background-color: white;">
        <p class="pt-4 mb-3 ms-4 text-primary"><i class="fa-solid fa-file-invoice"></i> Sử dụng mã giảm giá</p>
        <div class="container">
          <ul class="list-group list-group-flush">
            <li class="list-group-item"><b>Tổng tiền:</b> <b class="float-end text-danger">2.580.000 <sup><u>đ</u></sup></b></li>
            <li class="list-group-item">Điểm tích lũy quà tặng vip: <p class="float-end">102.900 điểm</p></li>
          </ul>
        </div>
        <div class="form-check ms-4">
          <input class="form-check-input" type="checkbox" id="flexCheckDefault">
          <label class="form-check-label" for="flexCheckDefault">
            Tôi đồng ý với <span class="text-primary ms">Chính sách xử lý dữ liệu cá nhân</span> của Rubik OCEAN
          </label>
        </div>
        <button type="button" class="btn btn-primary rounded-4 ms-4 mt-4" style="width: 600px; height: 50px;">Đặt hàng</button>
      </div>
    </div>
  </template>