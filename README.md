
# Vitis-Callback_ref
中断处理程序回调参数
假设我们有一个设备，需要在中断发生时处理一些操作，而这些操作需要知道设备的 ID 和一些状态信息。为了让中断处理函数知道这些信息，我们可以定义一个结构体来保存设备信息，并将该结构体的指针传递给中断处理程序。

    typedef struct {
    int device_id;  // 设备 ID
    int status;     // 

这里，我们定义了一个名为 `InterruptContext` 的结构体，它包含了设备的 `device_id` 和 `status`。
中断服务程序会接受一个指针参数 `callback_ref`，这个指针实际上指向了我们定义的 `InterruptContext` 结构体。在中断发生时，处理函数就可以通过这个指针来访问设备的状态信息。

    static void intr_handler(void *callback_ref) 
    {
        // 将 callback_ref 转换成 InterruptContext 类型的指针
        InterruptContext *context = (InterruptContext *)callback_ref;

        // 使用 context 访问结构体中的数据
        if (context->status == 1) 
        {
            // 设备状态为 1，执行某些操作
            printf("Device %d is active!\n", context->device_id);
        }
        else 
        {
            // 设备状态为其他值，执行其他操作
            printf("Device %d is inactive.\n", context->device_id);
        }
    }

这里，我们创建了一个 `InterruptContext` 实例 `context`，并将其指针传递给中断处理程序。`intr_handler` 在中断发生时，就可以通过 `callback_ref` 来访问并使用 `context` 中的数据。
