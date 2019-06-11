# [2019OS] Project1: Process Scheduling

## 設計

主架構基於助教提供的sample_code，細節部分參考了許多網路上的virtual_memory教程，在kernel層級實作device model並設計之中的各項方法，包含完整的fnctl以及mmap，完成後將整份文件編入kernel中。

在program的部分則新增mmap的測試，最後使用兩種方法進行檔案傳輸時間的比較。

## 測試結果與比較

我們對兩筆不同大小的檔案分別進行兩種傳輸方法。

輸入：

    ./slave received_file1 fcntl 127.0.0.1 & ./master ../data/file1_in fcntl
    ./slave received_file1 fcntl 127.0.0.1 & ./master ../data/file1_in mmap
    ./slave received_file2 fcntl 127.0.0.1 & ./master ../data/file2_in fcntl
    ./slave received_file2 fcntl 127.0.0.1 & ./master ../data/file2_in mmap

輸出：

    ioctl success
    Transmission time: 0.073800 ms, File size: 4 bytes
    Transmission time: 0.456300 ms, File size: 4 bytes
    [1]+  已完成             ./slave received_file1 fcntl 127.0.0.1

    ioctl success
    Transmission time: 0.075600 ms, File size: 4 bytes
    Transmission time: 0.053500 ms, File size: 4 bytes
    [1]+  已完成             ./slave received_file1 mmap 127.0.0.1

    ioctl success
    Transmission time: 11.506200 ms, File size: 1502860 bytes
    Transmission time: 17.651100 ms, File size: 1502860 bytes
    [1]+  已完成             ./slave received_file2 fcntl 127.0.0.1

    ioctl success
    Transmission time: 3.094700 ms, File size: 1502860 bytes
    Transmission time: 6.935600 ms, File size: 1502860 bytes
    [1]+  已完成             ./slave received_file2 mmap 127.0.0.1

可以看到bytes數小的時候，兩者差別不大，甚至fcntl的速度反超mmap，可以說在誤差範圍內。
而在bytes數大時，需要走訪過整段記憶體的fcntl就遠遠落後於mmap了。
之所以mmap的速度超過fcntl，是因為mmap只做一次syscall，使process中的記憶體與page cache中的記憶體相當，而不用像fcntl一樣去複製整段記憶體。

### 組員貢獻

架構設計：All  
coding：李豈翔、賴億泓、黃柏升、林震  
reference 提供: 黃柏升  
系統環境提供：顏楷侖  
github repository:林秉駿  
