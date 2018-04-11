1. 制作启动盘。

2. 以下有两种方式安装系统：一种硬盘格式是MBR，一种硬盘格式是GPT。

3. 硬盘格式化可以使用Windows自带的diskpart。

   ```markdown
   >>>diskpart
   >>>list disk
   >>>select disk[=n]
   >>>clean ;此步会格式化所选硬盘的所有数据！！！切记备份！！！
   >>>convert mbr ;将所选硬盘格式化为MBR格式
   or:
   >>>convert gpt ;将所选硬盘格式化为GPT格式--->推荐
   ```

4. 关于UEFI/Legacy和Boot security的选择：

   4.1.格式为MBR：

   ```markdown
   Boot security:Disable
   UEFI/Legacy:Legacy Only
   CSM:Yes
   ```

   4.2.格式为GPT：

   ```
   Boot security:Enable
   UEFI/Legacy:UEFI Only
   CSM:No
   ```