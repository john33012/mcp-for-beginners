<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "8da8a0fd44d58fab5979d0f2914a1f37",
  "translation_date": "2025-07-17T09:07:56+00:00",
  "source_file": "03-GettingStarted/02-client/README.md",
  "language_code": "id"
}
-->
# Membuat client

Client adalah aplikasi atau skrip kustom yang berkomunikasi langsung dengan MCP Server untuk meminta sumber daya, alat, dan prompt. Berbeda dengan menggunakan alat inspector yang menyediakan antarmuka grafis untuk berinteraksi dengan server, menulis client sendiri memungkinkan interaksi secara programatik dan otomatis. Ini memungkinkan pengembang mengintegrasikan kemampuan MCP ke dalam alur kerja mereka, mengotomatisasi tugas, dan membangun solusi kustom sesuai kebutuhan spesifik.

## Gambaran Umum

Pelajaran ini memperkenalkan konsep client dalam ekosistem Model Context Protocol (MCP). Kamu akan belajar cara menulis client sendiri dan menghubungkannya ke MCP Server.

## Tujuan Pembelajaran

Pada akhir pelajaran ini, kamu akan mampu:

- Memahami apa yang dapat dilakukan oleh client.
- Menulis client sendiri.
- Menghubungkan dan menguji client dengan MCP server untuk memastikan server berfungsi sesuai harapan.

## Apa saja yang perlu dilakukan untuk menulis client?

Untuk menulis client, kamu perlu melakukan hal-hal berikut:

- **Impor pustaka yang tepat**. Kamu akan menggunakan pustaka yang sama seperti sebelumnya, hanya saja dengan konstruksi yang berbeda.
- **Buat instance client**. Ini melibatkan pembuatan instance client dan menghubungkannya ke metode transportasi yang dipilih.
- **Tentukan sumber daya apa yang akan didaftarkan**. MCP server menyediakan sumber daya, alat, dan prompt, kamu perlu memutuskan mana yang akan didaftarkan.
- **Integrasikan client ke aplikasi host**. Setelah mengetahui kemampuan server, kamu perlu mengintegrasikan ini ke aplikasi host sehingga jika pengguna mengetik prompt atau perintah lain, fitur server yang sesuai akan dipanggil.

Setelah kita memahami secara garis besar apa yang akan dilakukan, mari kita lihat contoh berikut.

### Contoh client

Mari kita lihat contoh client berikut:

### TypeScript

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";

const transport = new StdioClientTransport({
  command: "node",
  args: ["server.js"]
});

const client = new Client(
  {
    name: "example-client",
    version: "1.0.0"
  }
);

await client.connect(transport);

// List prompts
const prompts = await client.listPrompts();

// Get a prompt
const prompt = await client.getPrompt({
  name: "example-prompt",
  arguments: {
    arg1: "value"
  }
});

// List resources
const resources = await client.listResources();

// Read a resource
const resource = await client.readResource({
  uri: "file:///example.txt"
});

// Call a tool
const result = await client.callTool({
  name: "example-tool",
  arguments: {
    arg1: "value"
  }
});
```

Dalam kode di atas kita:

- Mengimpor pustaka
- Membuat instance client dan menghubungkannya menggunakan stdio sebagai transport.
- Mendaftarkan prompt, sumber daya, dan alat lalu memanggil semuanya.

Itulah client yang dapat berkomunikasi dengan MCP Server.

Mari kita luangkan waktu di bagian latihan berikutnya untuk membahas setiap potongan kode dan menjelaskan apa yang terjadi.

## Latihan: Menulis client

Seperti yang sudah disebutkan, mari kita jelaskan kode ini dengan detail, dan silakan ikuti sambil coding jika ingin.

### -1- Mengimpor pustaka

Mari kita impor pustaka yang dibutuhkan, kita akan membutuhkan referensi ke client dan protokol transportasi yang dipilih, yaitu stdio. stdio adalah protokol untuk hal-hal yang dijalankan di mesin lokalmu. SSE adalah protokol transportasi lain yang akan kita tunjukkan di bab berikutnya, tapi itu adalah opsi lain. Untuk sekarang, mari lanjutkan dengan stdio.

### TypeScript

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";
```

### Python

```python
from mcp import ClientSession, StdioServerParameters, types
from mcp.client.stdio import stdio_client
```

### .NET

```csharp
using Microsoft.Extensions.AI;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol.Transport;
```

### Java

Untuk Java, kamu akan membuat client yang terhubung ke MCP server dari latihan sebelumnya. Menggunakan struktur proyek Java Spring Boot yang sama dari [Getting Started with MCP Server](../../../../03-GettingStarted/01-first-server/solution/java), buat kelas Java baru bernama `SDKClient` di folder `src/main/java/com/microsoft/mcp/sample/client/` dan tambahkan impor berikut:

```java
import java.util.Map;
import org.springframework.web.reactive.function.client.WebClient;
import io.modelcontextprotocol.client.McpClient;
import io.modelcontextprotocol.client.transport.WebFluxSseClientTransport;
import io.modelcontextprotocol.spec.McpClientTransport;
import io.modelcontextprotocol.spec.McpSchema.CallToolRequest;
import io.modelcontextprotocol.spec.McpSchema.CallToolResult;
import io.modelcontextprotocol.spec.McpSchema.ListToolsResult;
```

Mari lanjut ke instansiasi.

### -2- Membuat instance client dan transport

Kita perlu membuat instance transport dan client:

### TypeScript

```typescript
const transport = new StdioClientTransport({
  command: "node",
  args: ["server.js"]
});

const client = new Client(
  {
    name: "example-client",
    version: "1.0.0"
  }
);

await client.connect(transport);
```

Dalam kode di atas kita telah:

- Membuat instance transport stdio. Perhatikan bagaimana ia menentukan command dan args untuk menemukan dan menjalankan server karena ini perlu dilakukan saat membuat client.

    ```typescript
    const transport = new StdioClientTransport({
        command: "node",
        args: ["server.js"]
    });
    ```

- Membuat instance client dengan memberikan nama dan versi.

    ```typescript
    const client = new Client(
    {
        name: "example-client",
        version: "1.0.0"
    });
    ```

- Menghubungkan client ke transport yang dipilih.

    ```typescript
    await client.connect(transport);
    ```

### Python

```python
from mcp import ClientSession, StdioServerParameters, types
from mcp.client.stdio import stdio_client

# Create server parameters for stdio connection
server_params = StdioServerParameters(
    command="mcp",  # Executable
    args=["run", "server.py"],  # Optional command line arguments
    env=None,  # Optional environment variables
)

async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read, write
        ) as session:
            # Initialize the connection
            await session.initialize()

          

if __name__ == "__main__":
    import asyncio

    asyncio.run(run())
```

Dalam kode di atas kita telah:

- Mengimpor pustaka yang dibutuhkan
- Membuat objek parameter server yang akan digunakan untuk menjalankan server agar client dapat terhubung.
- Mendefinisikan metode `run` yang memanggil `stdio_client` untuk memulai sesi client.
- Membuat titik masuk di mana metode `run` dipanggil melalui `asyncio.run`.

### .NET

```dotnet
using Microsoft.Extensions.AI;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol.Transport;

var builder = Host.CreateApplicationBuilder(args);

builder.Configuration
    .AddEnvironmentVariables()
    .AddUserSecrets<Program>();



var clientTransport = new StdioClientTransport(new()
{
    Name = "Demo Server",
    Command = "dotnet",
    Arguments = ["run", "--project", "path/to/file.csproj"],
});

await using var mcpClient = await McpClientFactory.CreateAsync(clientTransport);
```

Dalam kode di atas kita telah:

- Mengimpor pustaka yang dibutuhkan.
- Membuat transport stdio dan client `mcpClient`. Client ini akan digunakan untuk mendaftar dan memanggil fitur di MCP Server.

Catatan, pada "Arguments", kamu bisa menunjuk ke *.csproj* atau ke executable.

### Java

```java
public class SDKClient {
    
    public static void main(String[] args) {
        var transport = new WebFluxSseClientTransport(WebClient.builder().baseUrl("http://localhost:8080"));
        new SDKClient(transport).run();
    }
    
    private final McpClientTransport transport;

    public SDKClient(McpClientTransport transport) {
        this.transport = transport;
    }

    public void run() {
        var client = McpClient.sync(this.transport).build();
        client.initialize();
        
        // Your client logic goes here
    }
}
```

Dalam kode di atas kita telah:

- Membuat metode main yang mengatur transport SSE mengarah ke `http://localhost:8080` tempat MCP server berjalan.
- Membuat kelas client yang menerima transport sebagai parameter konstruktor.
- Dalam metode `run`, kita membuat client MCP sinkron menggunakan transport dan menginisialisasi koneksi.
- Menggunakan transport SSE (Server-Sent Events) yang cocok untuk komunikasi berbasis HTTP dengan MCP server Java Spring Boot.

### -3- Mendaftar fitur server

Sekarang kita sudah punya client yang bisa terhubung jika program dijalankan. Namun, client belum menampilkan fitur-fiturnya, mari lakukan itu sekarang:

### TypeScript

```typescript
// List prompts
const prompts = await client.listPrompts();

// List resources
const resources = await client.listResources();

// list tools
const tools = await client.listTools();
```

### Python

```python
# List available resources
resources = await session.list_resources()
print("LISTING RESOURCES")
for resource in resources:
    print("Resource: ", resource)

# List available tools
tools = await session.list_tools()
print("LISTING TOOLS")
for tool in tools.tools:
    print("Tool: ", tool.name)
```

Di sini kita mendaftar sumber daya yang tersedia dengan `list_resources()` dan alat dengan `list_tools` lalu mencetaknya.

### .NET

```dotnet
foreach (var tool in await client.ListToolsAsync())
{
    Console.WriteLine($"{tool.Name} ({tool.Description})");
}
```

Di atas adalah contoh bagaimana kita bisa mendaftar alat di server. Untuk setiap alat, kita cetak namanya.

### Java

```java
// List and demonstrate tools
ListToolsResult toolsList = client.listTools();
System.out.println("Available Tools = " + toolsList);

// You can also ping the server to verify connection
client.ping();
```

Dalam kode di atas kita telah:

- Memanggil `listTools()` untuk mendapatkan semua alat yang tersedia dari MCP server.
- Menggunakan `ping()` untuk memverifikasi koneksi ke server.
- `ListToolsResult` berisi informasi tentang semua alat termasuk nama, deskripsi, dan skema inputnya.

Bagus, sekarang kita sudah mendapatkan semua fitur. Pertanyaannya, kapan kita menggunakannya? Client ini cukup sederhana, artinya kita harus memanggil fitur secara eksplisit saat membutuhkannya. Di bab berikutnya, kita akan membuat client yang lebih canggih yang memiliki akses ke model bahasa besar (LLM) sendiri. Untuk sekarang, mari lihat bagaimana memanggil fitur di server:

### -4- Memanggil fitur

Untuk memanggil fitur, kita harus memastikan argumen yang benar diberikan dan dalam beberapa kasus nama fitur yang ingin dipanggil.

### TypeScript

```typescript

// Read a resource
const resource = await client.readResource({
  uri: "file:///example.txt"
});

// Call a tool
const result = await client.callTool({
  name: "example-tool",
  arguments: {
    arg1: "value"
  }
});

// call prompt
const promptResult = await client.getPrompt({
    name: "review-code",
    arguments: {
        code: "console.log(\"Hello world\")"
    }
})
```

Dalam kode di atas kita:

- Membaca sumber daya, kita memanggil sumber daya dengan `readResource()` dan menentukan `uri`. Ini kira-kira seperti ini di sisi server:

    ```typescript
    server.resource(
        "readFile",
        new ResourceTemplate("file://{name}", { list: undefined }),
        async (uri, { name }) => ({
          contents: [{
            uri: uri.href,
            text: `Hello, ${name}!`
          }]
        })
    );
    ```

    Nilai `uri` kita `file://example.txt` cocok dengan `file://{name}` di server. `example.txt` akan dipetakan ke `name`.

- Memanggil alat, kita memanggilnya dengan menentukan `name` dan `arguments` seperti ini:

    ```typescript
    const result = await client.callTool({
        name: "example-tool",
        arguments: {
            arg1: "value"
        }
    });
    ```

- Mendapatkan prompt, untuk mendapatkan prompt, kamu panggil `getPrompt()` dengan `name` dan `arguments`. Kode servernya seperti ini:

    ```typescript
    server.prompt(
        "review-code",
        { code: z.string() },
        ({ code }) => ({
            messages: [{
            role: "user",
            content: {
                type: "text",
                text: `Please review this code:\n\n${code}`
            }
            }]
        })
    );
    ```

    dan kode client yang dihasilkan akan seperti ini agar sesuai dengan deklarasi di server:

    ```typescript
    const promptResult = await client.getPrompt({
        name: "review-code",
        arguments: {
            code: "console.log(\"Hello world\")"
        }
    })
    ```

### Python

```python
# Read a resource
print("READING RESOURCE")
content, mime_type = await session.read_resource("greeting://hello")

# Call a tool
print("CALL TOOL")
result = await session.call_tool("add", arguments={"a": 1, "b": 7})
print(result.content)
```

Dalam kode di atas kita telah:

- Memanggil sumber daya bernama `greeting` menggunakan `read_resource`.
- Memanggil alat bernama `add` menggunakan `call_tool`.

### .NET

1. Mari tambahkan kode untuk memanggil alat:

  ```csharp
  var result = await mcpClient.CallToolAsync(
      "Add",
      new Dictionary<string, object?>() { ["a"] = 1, ["b"] = 3  },
      cancellationToken:CancellationToken.None);
  ```

1. Untuk mencetak hasilnya, berikut kode untuk menanganinya:

  ```csharp
  Console.WriteLine(result.Content.First(c => c.Type == "text").Text);
  // Sum 4
  ```

### Java

```java
// Call various calculator tools
CallToolResult resultAdd = client.callTool(new CallToolRequest("add", Map.of("a", 5.0, "b", 3.0)));
System.out.println("Add Result = " + resultAdd);

CallToolResult resultSubtract = client.callTool(new CallToolRequest("subtract", Map.of("a", 10.0, "b", 4.0)));
System.out.println("Subtract Result = " + resultSubtract);

CallToolResult resultMultiply = client.callTool(new CallToolRequest("multiply", Map.of("a", 6.0, "b", 7.0)));
System.out.println("Multiply Result = " + resultMultiply);

CallToolResult resultDivide = client.callTool(new CallToolRequest("divide", Map.of("a", 20.0, "b", 4.0)));
System.out.println("Divide Result = " + resultDivide);

CallToolResult resultHelp = client.callTool(new CallToolRequest("help", Map.of()));
System.out.println("Help = " + resultHelp);
```

Dalam kode di atas kita telah:

- Memanggil beberapa alat kalkulator menggunakan metode `callTool()` dengan objek `CallToolRequest`.
- Setiap panggilan alat menentukan nama alat dan `Map` argumen yang dibutuhkan alat tersebut.
- Alat server mengharapkan nama parameter tertentu (seperti "a", "b" untuk operasi matematika).
- Hasil dikembalikan sebagai objek `CallToolResult` yang berisi respons dari server.

### -5- Menjalankan client

Untuk menjalankan client, ketik perintah berikut di terminal:

### TypeScript

Tambahkan entri berikut ke bagian "scripts" di *package.json*:

```json
"client": "tsx && node build/client.js"
```

```sh
npm run client
```

### Python

Panggil client dengan perintah berikut:

```sh
python client.py
```

### .NET

```sh
dotnet run
```

### Java

Pastikan MCP server berjalan di `http://localhost:8080`. Kemudian jalankan client:

```bash
# Build you project
./mvnw clean compile

# Run the client
./mvnw exec:java -Dexec.mainClass="com.microsoft.mcp.sample.client.SDKClient"
```

Atau, kamu bisa menjalankan proyek client lengkap yang disediakan di folder solusi `03-GettingStarted\02-client\solution\java`:

```bash
# Navigate to the solution directory
cd 03-GettingStarted/02-client/solution/java

# Build and run the JAR
./mvnw clean package
java -jar target/calculator-client-0.0.1-SNAPSHOT.jar
```

## Tugas

Dalam tugas ini, kamu akan menggunakan apa yang sudah dipelajari untuk membuat client sendiri.

Berikut server yang bisa kamu gunakan dan harus dipanggil melalui kode client-mu, coba tambahkan lebih banyak fitur ke server agar lebih menarik.

### TypeScript

```typescript
import { McpServer, ResourceTemplate } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create an MCP server
const server = new McpServer({
  name: "Demo",
  version: "1.0.0"
});

// Add an addition tool
server.tool("add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

// Add a dynamic greeting resource
server.resource(
  "greeting",
  new ResourceTemplate("greeting://{name}", { list: undefined }),
  async (uri, { name }) => ({
    contents: [{
      uri: uri.href,
      text: `Hello, ${name}!`
    }]
  })
);

// Start receiving messages on stdin and sending messages on stdout

async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCPServer started on stdin/stdout");
}

main().catch((error) => {
  console.error("Fatal error: ", error);
  process.exit(1);
});
```

### Python

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")


# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b


# Add a dynamic greeting resource
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"

```

### .NET

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ModelContextProtocol.Server;
using System.ComponentModel;

var builder = Host.CreateApplicationBuilder(args);
builder.Logging.AddConsole(consoleLogOptions =>
{
    // Configure all logs to go to stderr
    consoleLogOptions.LogToStandardErrorThreshold = LogLevel.Trace;
});

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();
await builder.Build().RunAsync();

[McpServerToolType]
public static class CalculatorTool
{
    [McpServerTool, Description("Adds two numbers")]
    public static string Add(int a, int b) => $"Sum {a + b}";
}
```

Lihat proyek ini untuk cara [menambahkan prompt dan sumber daya](https://github.com/modelcontextprotocol/csharp-sdk/blob/main/samples/EverythingServer/Program.cs).

Juga, cek tautan ini untuk cara memanggil [prompt dan sumber daya](https://github.com/modelcontextprotocol/csharp-sdk/blob/main/src/ModelContextProtocol/Client/).

## Solusi

**Folder solusi** berisi implementasi client lengkap yang siap dijalankan dan mendemonstrasikan semua konsep yang dibahas dalam tutorial ini. Setiap solusi mencakup kode client dan server yang diorganisasi dalam proyek terpisah dan mandiri.

### 📁 Struktur Solusi

Direktori solusi diorganisasi berdasarkan bahasa pemrograman:

```
solution/
├── typescript/          # TypeScript client with npm/Node.js setup
│   ├── package.json     # Dependencies and scripts
│   ├── tsconfig.json    # TypeScript configuration
│   └── src/             # Source code
├── java/                # Java Spring Boot client project
│   ├── pom.xml          # Maven configuration
│   ├── src/             # Java source files
│   └── mvnw            # Maven wrapper
├── python/              # Python client implementation
│   ├── client.py        # Main client code
│   ├── server.py        # Compatible server
│   └── README.md        # Python-specific instructions
├── dotnet/              # .NET client project
│   ├── dotnet.csproj    # Project configuration
│   ├── Program.cs       # Main client code
│   └── dotnet.sln       # Solution file
└── server/              # Additional .NET server implementation
    ├── Program.cs       # Server code
    └── server.csproj    # Server project file
```

### 🚀 Apa yang Termasuk di Setiap Solusi

Setiap solusi spesifik bahasa menyediakan:

- **Implementasi client lengkap** dengan semua fitur dari tutorial
- **Struktur proyek yang berfungsi** dengan dependensi dan konfigurasi yang tepat
- **Skrip build dan run** untuk setup dan eksekusi mudah
- **README detail** dengan instruksi spesifik bahasa
- **Penanganan error** dan contoh pemrosesan hasil

### 📖 Menggunakan Solusi

1. **Masuk ke folder bahasa pilihanmu**:
   ```bash
   cd solution/typescript/    # For TypeScript
   cd solution/java/          # For Java
   cd solution/python/        # For Python
   cd solution/dotnet/        # For .NET
   ```

2. **Ikuti instruksi README** di setiap folder untuk:
   - Menginstal dependensi
   - Membangun proyek
   - Menjalankan client

3. **Contoh output** yang akan kamu lihat:
   ```text
   Prompt: Please review this code: console.log("hello");
   Resource template: file
   Tool result: { content: [ { type: 'text', text: '9' } ] }
   ```

Untuk dokumentasi lengkap dan instruksi langkah demi langkah, lihat: **[📖 Dokumentasi Solusi](./solution/README.md)**

## 🎯 Contoh Lengkap

Kami menyediakan implementasi client lengkap dan berfungsi untuk semua bahasa pemrograman yang dibahas dalam tutorial ini. Contoh ini menunjukkan seluruh fungsi yang dijelaskan di atas dan dapat digunakan sebagai referensi atau titik awal untuk proyekmu sendiri.

### Contoh Lengkap yang Tersedia

| Bahasa | File | Deskripsi |
|--------|------|-----------|
| **Java** | [`client_example_java.java`](../../../../03-GettingStarted/02-client/client_example_java.java) | Client Java lengkap menggunakan transport SSE dengan penanganan error menyeluruh |
| **C#** | [`client_example_csharp.cs`](../../../../03-GettingStarted/02-client/client_example_csharp.cs) | Client C# lengkap menggunakan transport stdio dengan startup server otomatis |
| **TypeScript** | [`client_example_typescript.ts`](../../../../03-GettingStarted/02-client/client_example_typescript.ts) | Client TypeScript lengkap dengan dukungan penuh protokol MCP |
| **Python** | [`client_example_python.py`](../../../../03-GettingStarted/02-client/client_example_python.py) | Client Python lengkap menggunakan pola async/await |

Setiap contoh lengkap mencakup:

- ✅ **Pembuatan koneksi** dan penanganan error
- ✅ **Penemuan server** (alat, sumber daya, prompt jika ada)
- ✅ **Operasi kalkulator** (tambah, kurang, kali, bagi, bantuan)
- ✅ **Pemrosesan hasil** dan output terformat
- ✅ **Penanganan error menyeluruh**
- ✅ **Kode bersih dan terdokumentasi** dengan komentar langkah demi langkah

### Memulai dengan Contoh Lengkap

1. **Pilih bahasa yang kamu inginkan** dari tabel di atas
2. **Tinjau file contoh lengkap** untuk memahami implementasi penuh
3. **Jalankan contoh** sesuai instruksi di [`complete_examples.md`](./complete_examples.md)
4. **Modifikasi dan kembangkan** contoh sesuai kebutuhanmu

Untuk dokumentasi lengkap tentang menjalankan dan menyesuaikan contoh ini, lihat: **[📖 Dokumentasi Contoh Lengkap](./complete_examples.md)**

### 💡 Solusi vs. Contoh Lengkap

| **Folder Solusi** | **Contoh Lengkap** |
|-------------------|--------------------|
| Struktur proyek lengkap dengan file build | Implementasi satu file |
| Siap dijalankan dengan dependensi | Contoh kode fokus |
| Setup seperti produksi | Referensi edukasi |
| Tooling spesifik bahasa | Perbandingan lintas bahasa |
Kedua pendekatan sama-sama berharga - gunakan **folder solution** untuk proyek lengkap dan **complete examples** untuk belajar dan referensi.  
## Poin Penting

Poin penting dari bab ini tentang klien adalah sebagai berikut:

- Dapat digunakan untuk menemukan dan memanggil fitur di server.  
- Dapat memulai server saat klien itu sendiri berjalan (seperti di bab ini), tapi klien juga bisa terhubung ke server yang sudah berjalan.  
- Merupakan cara yang bagus untuk menguji kemampuan server selain alternatif seperti Inspector yang dijelaskan di bab sebelumnya.  

## Sumber Tambahan

- [Building clients in MCP](https://modelcontextprotocol.io/quickstart/client)

## Contoh

- [Java Calculator](../samples/java/calculator/README.md)  
- [.Net Calculator](../../../../03-GettingStarted/samples/csharp)  
- [JavaScript Calculator](../samples/javascript/README.md)  
- [TypeScript Calculator](../samples/typescript/README.md)  
- [Python Calculator](../../../../03-GettingStarted/samples/python)  

## Selanjutnya

- Selanjutnya: [Creating a client with an LLM](../03-llm-client/README.md)

**Penafian**:  
Dokumen ini telah diterjemahkan menggunakan layanan terjemahan AI [Co-op Translator](https://github.com/Azure/co-op-translator). Meskipun kami berupaya untuk mencapai akurasi, harap diperhatikan bahwa terjemahan otomatis mungkin mengandung kesalahan atau ketidakakuratan. Dokumen asli dalam bahasa aslinya harus dianggap sebagai sumber yang sahih. Untuk informasi penting, disarankan menggunakan terjemahan profesional oleh manusia. Kami tidak bertanggung jawab atas kesalahpahaman atau penafsiran yang keliru yang timbul dari penggunaan terjemahan ini.