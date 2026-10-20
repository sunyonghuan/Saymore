# 测试运行说明

在仓库根目录、已安装项目依赖的 Python 环境中运行普通测试：

```powershell
python -B -m unittest discover -s tests -v
```

## 真实窗口测试

以下两组测试默认跳过，避免普通测试过程中弹窗、移动窗口或切换焦点：

- `test_capsule_window.py` 的 `CapsuleWindowTest`：在右下角显示测试缓存文字，验证工具栏操作后将窗口移到左上角。
- `test_capsule.py` 的 `NativeCapsuleTest`：创建原生文字窗口，验证排版、滚动和编辑行为。

它们只在 Windows 下、环境变量 `SAYMORE_RUN_GUI_TESTS` 的值严格为 `1` 时运行。
需要人工验收界面时，在独立的 PowerShell 终端执行：

```powershell
$env:SAYMORE_RUN_GUI_TESTS = "1"
try {
    python -B -m unittest discover -s tests -p "test_capsule*.py" -v
} finally {
    Remove-Item Env:\SAYMORE_RUN_GUI_TESTS -ErrorAction SilentlyContinue
}
```

启用后会出现真实测试窗口，并可能短暂切换前台焦点。测试使用假的缓存和发送回调，
不调用语音模型，也不向外部输入框发送文字。

`test_capsule.py` 的 `MorphTest` 不创建桌面窗口，继续默认运行。
该开关仅控制测试，不影响应用正常的悬浮窗和文字暂存功能。
