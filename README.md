# http_servers
A micro project demonstrating the behaviour of an http server.

## Motivation
To use as a learning tool, to better understand the basics of http requests and responses.

## Build status
Finsihed.

## Screenshots

## Tech/framework used
Ruby with socket gem.

## Code Example
```Ruby
def run_server
  server = TCPServer.new(2345)

  loop do
    socket = server.accept

    request = get_lines_until_blank_line(socket)
    puts "=== GOT REQUEST ==="
    puts request
    puts "=== END REQUEST ==="

    if request.start_with? "GET / HTTP/1.1"
      response = "Hello World!\n"
    elsif request.start_with? "GET /secret_page HTTP/1.1"
      response = "1. The gold is buried at harald creek\n"
      response += "2. OK I only have one secret"
    else
      response = "Nothing found :("
    end

    http_response = build_http_response(response)
    socket.print http_response

    puts "=== SENT RESPONSE ==="
    puts http_response
    puts "=== END RESPONSE ==="
    puts "\n"

    socket.close
  end
end

def get_lines_until_blank_line(socket)
  message = ""
  loop do
    line = socket.gets
    if line.chomp == ""
      break
    else
      message += line
    end
  end
  message
end

def build_http_response(response)
  "HTTP/1.1 200 OK\r\n" +
    "Content-Type: text/plain\r\n" +
    "Content-Length: #{response.bytesize}\r\n" +
    "Connection: close\r\n" +
    "\r\n" +
    response
end
```

## How to use?
Run in terminal with `ruby http_server.rb` and then visit 'localhost:2345' in your browser.
