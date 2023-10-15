The point of this question was to invert the following 4x4 matrix, given that it consists of a rotation plus a translation:

> ```
> [ux vx wx tx]
> [uy vy wy ty]
> [uz vz wz tz]
> [ 0  0  0  1]
> ```

The matrix shown could be split into two matrices: a rotation matrix and a translation matrix. The order of the two matrices after the split was important:

> ```
> [ux vx wx tx]   [1 0 0 tx]   [ux vx wx 0]
> [uy vy wy ty]   [0 1 0 ty]   [uy vy wy 0]
> [uz vz wz tz] = [0 0 1 tz] * [uz vz wz 0]
> [ 0  0  0  1]   [0 0 0  1]   [ 0  0  0 1]
> ```

Given the original matrix split into two pieces, it is relatively straightforward to invert the matrix product if you remembered three general ideas:

1. The inverse of a translation matrix is the translation matrix with the opposite signs on each of the translation components.
2. The inverse of a rotation matrix is the rotation matrix's transpose.
3. The inverse of a matrix product is the product of the inverse matrices ordered in reverse.

Given these, the inverse of the matrix is found as follows:

> ```
> [ux vx wx tx] -1   ( [1 0 0 tx]   [ux vx wx 0] ) -1
> [uy vy wy ty]      ( [0 1 0 ty]   [uy vy wy 0] )
> [uz vz wz tz]    = ( [0 0 1 tz] * [uz vz wz 0] )
> [ 0  0  0  1]      ( [0 0 0  1]   [ 0  0  0 1] )
> 
>                 [ux vx wx 0] -1   [1 0 0 tx] -1
>                 [uy vy wy 0]      [0 1 0 ty]
>               = [ux vz wz 0]    * [0 0 1 tz]
>                 [ 0  0  0 1]      [0 0 0  1]
> 
>                 [ux uy uz 0]   [1 0 0 -tx]
>                 [vx vy vz 0]   [0 1 0 -ty]
>               = [wx wy wz 0] * [0 0 1 -tz]
>                 [ 0  0  0 1]   [0 0 0  1 ]
> 
>                 [ux uy uz -ux*tx-uy*ty-uz*tz]
>                 [vx vy vz -vx*tx-vy*ty-vz*tz]
>               = [wx wy wz -wx*tx-wy*ty-wz*tz]
>                 [ 0  0  0          1        ]
> 
>                 [ux uy uz -dot(u,t)]
>                 [vx vy vz -dot(v,t)]
>               = [wx wy wz -dot(w,t)]
>                 [ 0  0  0     1    ]
> ```


            ego2camera_r = np.linalg.inv(camera2ego[:3, :3])
            # 等价于
            ego2camera_t = -camera2ego[:3, 3]
            ego2camera = np.eye(4)
            ego2camera[:3,:3] = ego2camera_r
            ego2camera[:3,3] = ego2camera_r @ ego2camera_t
            
            # 等价于
            ego2camera = np.linalg.inv(camera2ego)


![image-20231015164723351](assets/inverse_rt/image-20231015164723351.png)

