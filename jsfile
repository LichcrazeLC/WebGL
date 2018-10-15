"use strict";

var gl;
var i = 0;

var thetaLoc;

var objects = [];
var obj_buttons_array = [];

var obj_index = 1;
var currently_selected_index = 0;

var rotating = false;

var globalPosition;
var globalScale;

function get_random_nr() {
    return Math.random();
}

function create_button_for_object() {

    var btn = document.createElement("button");
    var t = document.createTextNode("Object " + obj_index.toString());
    var index_stored = obj_index - 1;
    btn.appendChild(t);
    document.body.appendChild(btn);
    obj_index++;
    obj_buttons_array.push(btn);
    btn.classList.add("right_menu_buttons");
    btn.style.top = ((obj_index + 3) * 25 - 80).toString() + "px";
    btn.onclick = function () { objects[currently_selected_index].selected = false; currently_selected_index = index_stored; objects[currently_selected_index].selected = true; };

}

window.onload = function init() {
    var canvas = document.getElementById("gl-canvas");

    gl = WebGLUtils.setupWebGL(canvas);
    if (!gl) { alert("WebGL isn't available"); }

    gl.viewport(0, 0, canvas.width, canvas.height);
    gl.clearColor(1.0, 1.0, 1.0, 1.0);

    var program = initShaders(gl, "vertex-shader", "fragment-shader");

    gl.useProgram(program);

    var v_Position = gl.getAttribLocation(program, "v_Position");
    var colorPosition = gl.getAttribLocation(program, "a_color");
    
    thetaLoc = gl.getUniformLocation(program, "theta");
    globalPosition = gl.getUniformLocation(program, "tr");
    globalScale = gl.getUniformLocation(program, "scale");

    function selectPointer(){

        this.translation = 0;
        
        this.center_matrix = [0, 0, 0];

        this.swayup = true;
        this.pendulumcounter = 0;

        this.select_color = vec4(0, 0, 1, 0.1);

        this.selected = false;

        this.offset = 0.2;

        this.txAxis = 0;
        this.tyAxis = 1;
        this.tzAxis = 2;
        this.taxis = this.txAxis;

        this.theta = [0, 0, 0];
        this.xAxis = 0;
        this.yAxis = 1;
        this.zAxis = 2;
        this.axis = this.xAxis;

        this.sxAxis = 0;
        this.syAxis = 1;
        this.szAxis = 2;
        this.saxis = this.sxAxis;
        this.scale_x = 1;
        this.scale_y = 1;
        this.scale_z = 1;
        this.scale_matrix = [this.scale_x, this.scale_y, this.scale_z];
        this.scale = 1;

        this.rotating = false;
        this.vertices = [];
        this.colors = [];
        this.rotation = 0;
        this.translation_x = 0;
        this.translation_y = 0;
        this.translation_z = 0;
        this.scaleparam = 0;
        this.translation_matrix = [this.translation_x, this.translation_y, this.translation_z];


        this.vertices.push(vec3(-0.015, 0.025, 0));
        this.vertices.push(vec3(0.015, 0.025, 0));
        this.vertices.push(vec3(0, 0, 0));

        this.colors.push(vec4(1, 0, 0, 1));
        this.colors.push(vec4(1, 0, 0, 1));
        this.colors.push(vec4(1, 0, 0, 1));

        
        this.render = function () {

            var buffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.vertices), gl.STATIC_DRAW);

            gl.vertexAttribPointer(v_Position, 3, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(v_Position);

            var colorBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.colors), gl.STATIC_DRAW);

            gl.vertexAttribPointer(colorPosition, 4, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(colorPosition);
                
            this.theta[this.axis] = this.rotation;
            gl.uniform3fv(thetaLoc, this.theta);

            this.scale_matrix[this.saxis] = this.scale;
            gl.uniform3fv(globalScale, this.scale_matrix);

            if (objects.length >= 1){
                var selectedobj = objects[currently_selected_index];
                this.translation_x = selectedobj.translation_matrix[0];
                this.translation_z = selectedobj.translation_matrix[2];
                if (this.pendulumcounter <= 0.06 && this.swayup){
                    this.pendulumcounter += 0.003;
                }  else {
                    this.swayup = false;
                }

                if (!this.swayup && this.pendulumcounter >= 0){
                    this.pendulumcounter -= 0.003;
                } else {
                    this.swayup = true;
                }

                this.translation_y = parseFloat(selectedobj.translation_matrix[1]) + this.offset + this.pendulumcounter;
                this.translation_matrix = [this.translation_x, this.translation_y, this.translation_z];   

            }

            gl.uniform3fv(globalPosition, this.translation_matrix);  

            gl.drawArrays(gl.TRIANGLES, 0, this.vertices.length);

        }


    }

    function cube(x, y, z, scl) {  

        this.translation = 0;
        
        this.center_matrix = [0, 0, 0];

        this.select_color = vec4(0, 0, 1, 0.1);

        this.selected = false;

        this.txAxis = 0;
        this.tyAxis = 1;
        this.tzAxis = 2;
        this.taxis = this.txAxis;

        this.theta = [45.0, 45.0, 45.0];
        this.xAxis = 0;
        this.yAxis = 1;
        this.zAxis = 2;
        this.axis = this.xAxis;

        this.sxAxis = 0;
        this.syAxis = 1;
        this.szAxis = 2;
        this.saxis = this.sxAxis;
        this.scale_x = 1;
        this.scale_y = 1;
        this.scale_z = 1;
        this.scale_matrix = [this.scale_x, this.scale_y, this.scale_z];
        this.scale = 1;

        this.rotating = false;
        this.vertices = [];
        this.colors = [];
        this.rotation = 0;
        this.translation_x = x;
        this.translation_y = y;
        this.translation_z = z;
        this.scaleparam = scl;
        this.translation_matrix = [this.translation_x, this.translation_y, this.translation_z];

        this.bottom_left_corner = vec3(this.translation_x, this.translation_y, this.translation_z);
        this.top_left_corner = vec3(this.translation_x, this.translation_y + this.scaleparam, this.translation_z);
        this.bottom_right_corner = vec3(this.translation_x + this.scaleparam, this.translation_y, this.translation_z);
        this.top_right_corner = vec3(this.translation_x + this.scaleparam, this.translation_y + this.scaleparam, this.translation_z);

        this.top_right_corner_2 = vec3(this.translation_x + this.scaleparam, this.translation_y + this.scaleparam, this.translation_z + this.scaleparam);
        this.bottom_right_corner_2 = vec3(this.translation_x + this.scaleparam, this.translation_y, this.translation_z + this.scaleparam);
        this.bottom_left_corner_2 = vec3(this.translation_x, this.translation_y, this.translation_z + this.scaleparam);
        this.top_left_corner_2 = vec3(this.translation_x, this.translation_y + this.scaleparam, this.translation_z + this.scaleparam);

        this.draw = function () {

            this.vertices.push(this.bottom_left_corner);
            this.vertices.push(this.top_left_corner);
            this.vertices.push(this.bottom_right_corner);
            this.vertices.push(this.top_left_corner);
            this.vertices.push(this.bottom_right_corner);
            this.vertices.push(this.top_right_corner);

            this.vertices.push(this.bottom_left_corner_2);
            this.vertices.push(this.top_left_corner_2);
            this.vertices.push(this.bottom_right_corner_2);
            this.vertices.push(this.top_left_corner_2);
            this.vertices.push(this.bottom_right_corner_2);
            this.vertices.push(this.top_right_corner_2);

            this.vertices.push(this.bottom_left_corner);
            this.vertices.push(this.bottom_right_corner);
            this.vertices.push(this.bottom_right_corner_2);
            this.vertices.push(this.bottom_left_corner_2);
            this.vertices.push(this.bottom_left_corner);
            this.vertices.push(this.bottom_right_corner_2);

            this.vertices.push(this.top_left_corner);
            this.vertices.push(this.top_right_corner);
            this.vertices.push(this.top_right_corner_2);
            this.vertices.push(this.top_left_corner_2);
            this.vertices.push(this.top_left_corner);
            this.vertices.push(this.top_right_corner_2);

            this.vertices.push(this.bottom_left_corner);
            this.vertices.push(this.bottom_left_corner_2);
            this.vertices.push(this.top_left_corner);
            this.vertices.push(this.top_left_corner_2);
            this.vertices.push(this.bottom_left_corner_2);
            this.vertices.push(this.top_left_corner);

            this.vertices.push(this.bottom_right_corner);
            this.vertices.push(this.bottom_right_corner_2);
            this.vertices.push(this.top_right_corner);
            this.vertices.push(this.top_right_corner_2);
            this.vertices.push(this.bottom_right_corner_2);
            this.vertices.push(this.top_right_corner);

            for (i = 0; i <= 35; i++) {
                var color = vec4(get_random_nr(), get_random_nr(), get_random_nr(), 1);
                this.colors.push(color);
            }

            create_button_for_object();

        }

        this.render = function () {

            var buffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.vertices), gl.STATIC_DRAW);

            gl.vertexAttribPointer(v_Position, 3, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(v_Position);

            var colorBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.colors), gl.STATIC_DRAW);

            gl.vertexAttribPointer(colorPosition, 4, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(colorPosition);

            if (this.rotating){
               this.rotation += 1;
            }   

                gl.uniform3fv(globalPosition, this.center_matrix);          

                this.theta[this.axis] = this.rotation;
                gl.uniform3fv(thetaLoc, this.theta);

                this.scale_matrix[this.saxis] = this.scale;
                gl.uniform3fv(globalScale, this.scale_matrix);

                this.translation_matrix[this.taxis] = this.translation;
                gl.uniform3fv(globalPosition, this.translation_matrix);  

                gl.drawArrays(gl.TRIANGLES, 0, this.vertices.length);


        }
    }

    function cylinder(x, y, z, r) {

        this.theta = [45.0, 45.0, 45.0];

        this.center_matrix = [0, 0, 0];

        this.txAxis = 0;
        this.tyAxis = 1;
        this.tzAxis = 2;
        this.taxis = this.txAxis;

        this.xAxis = 0;
        this.yAxis = 1;
        this.zAxis = 2;
        this.axis = this.xAxis;

        this.sxAxis = 0;
        this.syAxis = 1;
        this.szAxis = 2;
        this.saxis = this.sxAxis;
        this.scale_x = 1;
        this.scale_y = 1;
        this.scale_z = 1;
        this.scale_matrix = [this.scale_x, this.scale_y, this.scale_z];
        this.scale = 1;

        this.translation = 0;

        this.rotating = false;
        this.vertices = [];
        this.colors = [];
        this.rotation = 0;
        this.translation_x = x;
        this.translation_y = y;
        this.translation_z = z;
        this.translation_matrix = [this.translation_x, this.translation_y, this.translation_z];

        this.j = 0;
        this.center = vec3(this.translation_x, this.translation_y, this.translation_z);
        this.second_point = vec3(this.translation_x, this.translation_y + 0.15, this.translation_z);
        this.color = vec4(get_random_nr(), get_random_nr(), get_random_nr(), 1);
        this.r = r;

        this.draw = function () {

            for (i = 0; i <= 200; i += 1) {

                this.j++;

                if (this.j > 25) {
                    this.color = vec4(get_random_nr(), get_random_nr(), get_random_nr(), 1);
                    this.j = 0;
                }

                this.vertices.push(this.center);
                this.vertices.push(this.second_point);
                this.vertices.push(vec3(this.r * Math.cos(i * 2 * Math.PI / 200) + this.translation_x, this.translation_y, this.r * Math.sin(i * 2 * Math.PI / 200) + this.translation_z));
                this.vertices.push(this.second_point);
                this.vertices.push(vec3(this.r * Math.cos(i * 2 * Math.PI / 200) + this.translation_x, this.translation_y + 0.15, this.r * Math.sin(i * 2 * Math.PI / 200) + this.translation_z));
                this.vertices.push(vec3(this.r * Math.cos(i * 2 * Math.PI / 200) + this.translation_x, this.translation_y, this.r * Math.sin(i * 2 * Math.PI / 200) + this.translation_z));
                this.colors.push(this.color);
                this.colors.push(this.color);
                this.colors.push(this.color);
                this.colors.push(this.color);
                this.colors.push(this.color);
                this.colors.push(this.color);

                this.vertices.push(vec3(this.r * Math.cos(i * Math.PI / 100) + this.translation_x, this.translation_y, this.r * Math.sin(i * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(vec3(this.r * Math.cos((i + 1) * Math.PI / 100) + this.translation_x, this.translation_y, this.r * Math.sin((i + 1) * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(this.center);
                this.colors.push(this.color);

                this.vertices.push(vec3(this.r * Math.cos(i * Math.PI / 100) + this.translation_x, this.translation_y + 0.15, this.r * Math.sin(i * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(vec3(this.r * Math.cos((i + 1) * Math.PI / 100) + this.translation_x, this.translation_y + 0.15, this.r * Math.sin((i + 1) * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(this.center);
                this.colors.push(this.color);   
            }

        }


        this.render = function () {

            var buffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.vertices), gl.STATIC_DRAW);

            gl.vertexAttribPointer(v_Position, 3, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(v_Position);

            var colorBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.colors), gl.STATIC_DRAW);

            gl.vertexAttribPointer(colorPosition, 4, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(colorPosition);

            if (this.rotating){
                this.rotation += 1;
            }

                gl.uniform3fv(globalPosition, this.center_matrix);          

                this.theta[this.axis] = this.rotation;
                gl.uniform3fv(thetaLoc, this.theta);

                this.scale_matrix[this.saxis] = this.scale;
                gl.uniform3fv(globalScale, this.scale_matrix);

        
                this.translation_matrix[this.taxis] = this.translation;
                gl.uniform3fv(globalPosition, this.translation_matrix);

            
            gl.drawArrays(gl.TRIANGLES, 0, this.vertices.length);

        }

        create_button_for_object();

    }

    function cone(x, y, z, r) {

        this.theta = [45.0, 45.0, 45.0];

        this.center_matrix = [0, 0, 0];

        this.xAxis = 0;
        this.yAxis = 1;
        this.zAxis = 2;
        this.axis = this.xAxis;

        this.txAxis = 0;
        this.tyAxis = 1;
        this.tzAxis = 2;
        this.taxis = this.txAxis;

        this.sxAxis = 0;
        this.syAxis = 1;
        this.szAxis = 2;
        this.saxis = this.sxAxis;
        this.scale_x = 1;
        this.scale_y = 1;
        this.scale_z = 1;
        this.scale_matrix = [this.scale_x, this.scale_y, this.scale_z];
        this.scale = 1;

        this.translation = 0;

        this.rotating = false;
        this.vertices = [];
        this.colors = [];
        this.rotation = 0;
        this.translation_x = x;
        this.translation_y = y;
        this.translation_z = z;
        this.translation_matrix = [this.translation_x, this.translation_y, this.translation_z];
  

        this.j = 0;
        this.center = vec3(this.translation_x, this.translation_y, this.translation_z);
        this.second_point = vec3(this.translation_x, this.translation_y + 0.15, this.translation_z);
        this.color = vec4(get_random_nr(), get_random_nr(), get_random_nr(), 1);
        this.r = r;

        this.draw = function () {

            for (i = 0; i <= 200; i += 1) {

                this.j++;
                if (this.j > 25) {
                    this.color = vec4(get_random_nr(), get_random_nr(), get_random_nr(), 1);
                    this.j = 0;
                }

                this.vertices.push(this.center);
                this.colors.push(this.color);
                this.vertices.push(this.second_point);
                this.colors.push(this.color);
                this.vertices.push(vec3(this.r * Math.cos(i * Math.PI / 100) + this.translation_x, this.translation_y, this.r * Math.sin(i * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);

                this.vertices.push(vec3(this.r * Math.cos(i * Math.PI / 100) + this.translation_x, this.translation_y, this.r * Math.sin(i * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(vec3(this.r * Math.cos((i + 1) * Math.PI / 100) + this.translation_x, this.translation_y, this.r * Math.sin((i + 1) * Math.PI / 100) + this.translation_z));
                this.colors.push(this.color);
                this.vertices.push(this.center);
                this.colors.push(this.color);

            }
        }

        this.render = function () {

            var buffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.vertices), gl.STATIC_DRAW);

            gl.vertexAttribPointer(v_Position, 3, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(v_Position);

            var colorBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
            gl.bufferData(gl.ARRAY_BUFFER, flatten(this.colors), gl.STATIC_DRAW);

            gl.vertexAttribPointer(colorPosition, 4, gl.FLOAT, false, 0, 0);
            gl.enableVertexAttribArray(colorPosition);

            if (this.rotating){
                this.rotation += 1;
            }

            gl.uniform3fv(globalPosition, this.center_matrix);          

            this.theta[this.axis] = this.rotation;
            gl.uniform3fv(thetaLoc, this.theta);
                 
            this.translation_matrix[this.taxis] = this.translation;
            gl.uniform3fv(globalPosition, this.translation_matrix);

            this.scale_matrix[this.saxis] = this.scale;
            gl.uniform3fv(globalScale, this.scale_matrix);

            gl.drawArrays(gl.TRIANGLES, 0, this.vertices.length);

        }

        create_button_for_object();
        

    }

    var get_cone_button = document.getElementById("GetCone");
    var get_cube_button = document.getElementById("GetCube");
    var get_cylinder_button = document.getElementById("GetCylinder");

    var pointer = new selectPointer();

    get_cone_button.onclick = function () {

        var newCone = new cone(0, 0, 0, 0.1);
        newCone.draw();
        objects.push(newCone);

    };

    get_cube_button.onclick = function () {

        var newCube = new cube(0, 0, 0, 0.1);
        newCube.draw();
        objects.push(newCube);

    };

    get_cylinder_button.onclick = function () {

        var newCylinder = new cylinder(0, 0, 0, 0.1);
        newCylinder.draw();
        objects.push(newCylinder);

    };

    
    var spinX_button = document.getElementById("SpinX");
    var spinY_button = document.getElementById("SpinY");
    var spinZ_button = document.getElementById("SpinZ");

    spinX_button.onclick = function () {objects[currently_selected_index].axis = objects[currently_selected_index].xAxis; objects[currently_selected_index].rotating = true; objects[currently_selected_index].rotation = 0;};
    spinY_button.onclick = function () {objects[currently_selected_index].axis = objects[currently_selected_index].yAxis; objects[currently_selected_index].rotating = true; objects[currently_selected_index].rotation = 0;};
    spinZ_button.onclick = function () {objects[currently_selected_index].axis = objects[currently_selected_index].zAxis; objects[currently_selected_index].rotating = true; objects[currently_selected_index].rotation = 0;};

    var sliderRotationx = document.getElementById("X_rotation_slider");
    sliderRotationx.oninput = function() {objects[currently_selected_index].axis = objects[currently_selected_index].xAxis; objects[currently_selected_index].rotating = false; objects[currently_selected_index].rotation = sliderRotationx.value;};

    var sliderRotationy = document.getElementById("Y_rotation_slider");
    sliderRotationy.oninput = function() {objects[currently_selected_index].axis = objects[currently_selected_index].yAxis; objects[currently_selected_index].rotating = false; objects[currently_selected_index].rotation = sliderRotationy.value;};

    var sliderRotationz = document.getElementById("Z_rotation_slider");
    sliderRotationz.oninput = function() {objects[currently_selected_index].axis = objects[currently_selected_index].zAxis; objects[currently_selected_index].rotating = false; objects[currently_selected_index].rotation = sliderRotationz.value;};

    var sliderx = document.getElementById("X_translation_slider");
    sliderx.oninput = function() {objects[currently_selected_index].taxis = objects[currently_selected_index].txAxis; objects[currently_selected_index].translation = sliderx.value;};

    var slidery = document.getElementById("Y_translation_slider");
    slidery.oninput = function() {objects[currently_selected_index].taxis = objects[currently_selected_index].tyAxis; objects[currently_selected_index].translation = slidery.value;};

    var sliderz = document.getElementById("Z_translation_slider");
    sliderz.oninput = function() {objects[currently_selected_index].taxis = objects[currently_selected_index].tzAxis; objects[currently_selected_index].translation = sliderz.value;};

    var sliderScalex = document.getElementById("X_scale_slider");
    sliderScalex.oninput = function() {objects[currently_selected_index].saxis = objects[currently_selected_index].sxAxis; objects[currently_selected_index].scale = sliderScalex.value;};

    var sliderScaley = document.getElementById("Y_scale_slider");
    sliderScaley.oninput = function() {objects[currently_selected_index].saxis = objects[currently_selected_index].syAxis; objects[currently_selected_index].scale = sliderScaley.value;};

    var sliderScalez = document.getElementById("Z_scale_slider");
    sliderScalez.oninput = function() {objects[currently_selected_index].saxis = objects[currently_selected_index].szAxis; objects[currently_selected_index].scale = sliderScalez.value;};



    function render_scene() {

        gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
        for (var i = 0; i < objects.length; i++) {
            objects[i].render();
        }
        pointer.render();

        requestAnimFrame(render_scene);

    }

    render_scene();

};










